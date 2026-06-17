# Navigation Architecture

SwissPass uses `go_router` with generated typed routes for compile-time safe navigation.

## Why this exists

- Removes string-based route names and map-based route arguments.
- Makes route navigation refactor-safe with generated route classes.
- Keeps the app ready for deep links and declarative redirects.

## Package stack

- `go_router: ^17.3.0`
- `go_router_builder: ^4.3.0`
- `build_runner: ^2.4.0`

## File layout

- `mobile/lib/navigation/app_routes.dart`: Typed route definitions.
- `mobile/lib/navigation/app_routes.g.dart`: Generated route code (do not edit manually).
- `mobile/lib/navigation/app_router.dart`: `GoRouter` instance for `MaterialApp.router`.
- `mobile/lib/navigation/quiz_route_arguments.dart`: Typed quiz entry arguments.
- `mobile/lib/navigation/quiz_result_route_arguments.dart`: Typed quiz result payload.

## Add a new route

1. Add a new `GoRouteData` class to `app_routes.dart`.
2. Run `fvm dart run build_runner build`.
3. Navigate with generated typed methods, for example `SomeRoute().push(context)`.

## Required patterns

- Use generated route classes for all navigation.
- Pass typed objects through route `extra` when a route needs payload data.
- Keep path values centralized in route data classes.

## Forbidden patterns

- `Navigator.pushNamed(...)`
- `Navigator.pushReplacementNamed(...)`
- `Navigator.pushNamedAndRemoveUntil(...)`
- `ModalRoute.of(context)?.settings.arguments`
- Passing route payloads as `Map<String, dynamic>`

