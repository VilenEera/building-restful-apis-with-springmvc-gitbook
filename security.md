#Secures APIs

We have configured Spring Secuirty in before posts. 

In this post, I will show you using Spring Secuirty to protect APIs, aka provides Anthentication and Anthorization service for this sample application.

* **Authentication** answers the question: if the user is a valid user.
* **Authorization** resolves the problem: if the authenticated user has corresponding permission to do something.

In Spring security, it is easy to configure JAAS compatible strategy, such as FORM, BASIC, X509 Certiciate etc. Unlike JAAS in which authentication management is very dependent on the container itself, Spring Security provides simple APIs(such as `UserDetails`, `UserDetailsService`, `Authority`) allow developers to implement them outside of a container, and control the authentication and authorization in a progammatic approach. 

Motioned in before posts, the simplest way to configure Spring security is using `AuthenticationManagerBuilder` to build essential required resources. 

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		http   
			.authorizeRequests()   
			.antMatchers("/api/ping")
			.permitAll()
		.and()
			.authorizeRequests()   
			.antMatchers("/api/**")
			.authenticated()
		.and()
			.authorizeRequests()   
			.anyRequest()
			.permitAll()
			.and()
				.sessionManagement()
				.sessionCreationPolicy(SessionCreationPolicy.STATELESS)
			.and()
				.httpBasic()
			.and()
				.csrf()
				.disable();
	}

	@Override
	protected void configure(AuthenticationManagerBuilder auth)
			throws Exception {
		auth.inMemoryAuthentication()
				.passwordEncoder(passwordEncoder())
				.withUser("admin").password("test123").authorities("ROLE_ADMIN")
				.and()
					.withUser("test").password("test123").authorities("ROLE_USER");
	}
	
An in-memory database and a HTTP BASIC authentication is easy to prototype applications, as showing as above codes. 

If you want to store users into your database, firstly create a custom `UserDetailsService` and implement the `findByUsername` method and return a `UserDetails` object.

	public class SimpleUserDetailsServiceImpl implements UserDetailsService {

		private static final Logger log = LoggerFactory.getLogger(SimpleUserDetailsServiceImpl.class);

		private UserRepository userRepository;

		public SimpleUserDetailsServiceImpl(UserRepository userRepository) {
			this.userRepository = userRepository;
		}

		@Override
		public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
			User user = userRepository.findByUsername(username);
			if (user == null) {
				throw new UsernameNotFoundException("username not found:" + username);
			}

			log.debug("found by username @" + username);

			return user;

		}

	}

Then replace above `inMemoryAuthentication` configuration with `UserDetailsService`.	

	auth.userDetailsService(new SimpleUserDetailsServiceImpl(userRepository))
		.passwordEncoder(passwordEncoder);
	
If you want to design a customized Authentication strategy, you could have to create a custom `AuthenticationEntryPoint` and `AuthenticationProvider` for it. We will discuss this later.

### Declarative URL pattern based authorizations

For REST APIs, the API resources are identified by URI, it is easy to grant authorizations via URL.

Override the `configure(HttpSecurity)` of `WebSecurityConfigurerAdapter`.

	public class SecurityConfig extends WebSecurityConfigurerAdapter {
	
		@Override
		protected void configure(HttpSecurity http) throws Exception {
			http   
				.authorizeRequests()   
				.antMatchers("/api/ping")
				.permitAll()
			.and()
				.authorizeRequests()   
				.antMatchers("/api/**")
				.authenticated()
				//....
		}

The access control is filter by the `Matcher`, there are two built-in matchers, Apache Ant path matcher, and perl like regex matchers. The later is a little complex, but more powerful. 	

`http...antMatchers("/api/**").authenticated()` means all resource URLs match '/api/**' need a valid authentication.

`http...antMatchers(HttpMethod.POST, "/api/posts").hasRoles("ADMIN")` indicates only users that have been granted **ADMIN** role have permission to create a new post. 

Combined with resource URLs and HTTP methods, it follows rest convention exactly.

In the a real world application, you can centralize the *URL pattern*, *HTTP Method*, and granted *ROLES* into a certain persistent storage(such as RDBMS or NOSQL) and desgin a friendly web UI to control resource access. 
		
### Method level authorizations 

Like JAAS, Spring Security provides several annotations to authorize access on method level.

Firstly you should add `@EnableGlobalMethodSecurity` on `@Configuration` class to enable it.

	
	@Configuration
	@EnableWebSecurity
	@EnableGlobalMethodSecurity(prePostEnabled = true, jsr250Enabled = true, securedEnabled = true)
	public class SecurityConfig extends WebSecurityConfigurerAdapter {
	
	}

If **prePostEnabled** is true, the `@PreAuthorized` and `@PostAuthorized` can be used, it accept Spring EL string and evaluate the result.

For example, only *ADMIN* user can save post.
	
	@PreAuthorized("hasRole('ADMIN')")
	public void savePost(Post post){}
	
And only the post owner can update the post.

	@PreAuthorized("#post.author.id==principal.id")
    public void update(Post post){}
	
**jsr250Enabled** provides Java common anntotation compatibility, and allow you use JAAS annotations in Spring project.
 	
**securedEnabled** enables the legacy `@Secured` annotation which does not accept Spring EL as property value.

	@Secured("ROLE_TELLER")	
	public void savePost(Post post){}
	
### Programmatic authorizations

Spring provides APIs to fetech current principal info. 

For example, get current Authetication from SecurityContextHolder.

	Authentication auth = SecurityContextHolder.getContext().getAuthentication();

And you can also inject current authenticated Principal like this.

	publc List<Post> getByCurrentUser(@AuthenticationPrincipal Principal principal){}

After got the security principal info, you can control the authorizations in codes.
	