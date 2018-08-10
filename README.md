# ThisTrip 

An in-the-moment reference for MetroTransit bus and rail services. This project contains the GraphQL API and React web interface for the project.

I ended up having a slight emphasis on the backend and data layer of the frontend application.

## Getting Started

The API and frontend are in separate directories (`api` and `frontend` respectively). 

In each of these directories, use `yarn` to install packages, and start the server with `yarn start`.

Tests can be run with `yarn test`, and type checking with `yarn flow`. Tests and type checking are run before every commit using `lint-staged`.

## Technologies and Tools Used

### Backend
The project's API is bundled with `parcel`, and makes use of `jest` for testing, and `flow` for type checking. I did not use generators or boilerplate projects to begin this portion of the project, as I wanted to keep the needs here relatively minimal.

The APIs I used are described on [MetroTransit's API documentation site](http://svc.metrotransit.org/). For certain situations, it was necessary to pre-load or store certain data that MetroTransit does not provide through their API endpoint (instead, this data originates from [a ZIP of CSV files](https://gisdata.mn.gov/dataset/us-mn-state-metc-trans-transit-schedule-google-fd) that is generated weekly, and imported into a Redis cache using the scripts in `api/src/script`). A copy of the data used can be found at `api/src/data/*.csv`.

For testing, I often refer to [this Hacker Noon](https://hackernoon.com/extensive-graphql-testing-57e8760f1c25) article for structural guidance, though generally resolver testing follows a similar approach across projects (whether using apollo-server on Node, `graphql-rails` on Ruby/Rails, or other platforms) in my experience.

### Frontend
This project's frontend was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app), which provided a solid foundation of bundler configuration and other environmental support, nicely encapsulated in the `react-scripts` package. I quickly removed the generated code, save for `index.js`, which handles the app's initial mounting and rendering.

## Caveats and constraints

As I elected to focus more heavily on the backend, UI styles and component-level testing could be vastly improved. While working on this, I built up a backlog of "nice-to-haves" that I would've also liked to use, including the option for users to identify preferred routes or stops that could be highlighted by the application. Combined with the use of Google Maps APIs, this could afford users a way to set "home" and "work" locations (for example), and rely on ThisTrip to identify nearby stops with buses that would take them to their home/office. I've often wished for such an app, myself, as a frequent transit user.

Due to time constraints, a strong reliance is placed on the type system to validate correctness of the code, and the use of proper property accesses. This leans on the strengths of `flow` in comparison to writing out equivalent specs in `jest` (which may look rather similar to methodically checking the existence and values of certain properties on resulting objects), however as the application grows there could be additional business logic imbued in those transformations that could necessitate more extensive testing. 

Flow may provide some assurances that the values we're operating on have an expected type, but it can't help guard against data that is invalid, incorrect or useless. With timeline in mind, I elected to focus the Jest specs on cases where invalid data is in place. With more time, a greater emphasis on integration testing would be a high priority.

Additionally, with more time, there could be a greater degree of shared code across the web interface and the API. While the Flow types are shared across the project, keeping them in sync would be cleaner if both the `web` and `api` projects used a third `types` module. For ease of development, I did not create a separate package for this. There are a number of opportunities to ensure the Flow types and GraphQL schemas remain in sync as well, which would be another focus for future work.
