
## Code Structure
- .Api : api related project
- .core, .dto etc are libraries used in .api
- .Analytics.Api : for supplier analytics
- .Web: frontend app
- .Worker: Azure function to receive service bus events for the app

### .Web
.Web/ClientApp/src/app: angular components and pages
project-management: buyer page
supplier-gateway: supplier page
