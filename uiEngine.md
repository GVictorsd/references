
# UI Engine
(podium.lib-npm-ui)

# Storybook
## Redux dynamic module

## Microfrontends

### Monorepos
- Monoreps: dev strategy where code from many projects are stored in the same repo
- individual projects have saperate lifecycles unlike a monolith app
- popular tools for monorepos:
- Lerna, Nrwl, Rush, Yarn Workspaces, Bazel

### disadvantages
- size of the code becomes huge 
- operational complexity since multiple teams contribute to same repo
- payload size is more since multiple features may require different versions of same library leading to longer load times
- deployment is completly different process