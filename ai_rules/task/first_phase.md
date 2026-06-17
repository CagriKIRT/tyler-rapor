Create a backend, mobile and launch_website folders.

##BACKEND
- For backend create a nestJS project with fastify adapter  that uses typscript.
- create dev and prod env for backend.I am using coolify in my server. Consider that when you create these env.
- add swagger that i can also in my local.


  
- create dev and prod env for backend and mobile. Use flavors when creating env. in mobile. I am using coolify in my server. Consider that when you create these env.

- create launch and settings json to top of the folder tree. These files cover for mobile and backend.

##Mobile
- For mobile create a flutter project with fvm using latest flutter version
- create dev and prod env  and mobile. Use flavors when creating env. in mobile. I am using coolify in my server. Consider that when you create these env.
- implement below dependency that you will use later when implementing base features.
  core:
    git:
     url: git@github.com-cagri:CagriKIRT/flutter_core.git
     ref: master
- Use `go_router: ^17.3.0` for all app navigation. Do not use Navigator 1.0 named route strings.
- Use `go_router_builder` to generate typed `GoRouteData` classes and call generated navigation methods.
- Route payloads must be strongly typed classes passed via route `extra`; do not pass `Map<String, dynamic>` route arguments.


