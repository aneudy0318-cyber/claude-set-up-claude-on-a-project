Simple Express.js user API with file-based storage.

Commands
- npm run dev - start development server with nodemon
- npm test - run Jest tests
- npm run lint - check code style

Conventions
- Use CommonJS require, not ESM import
- All route handlers must validate input and return JSON
- Data access only through db/store.js, never direct file access in routes

Architecture
- Entry point is server.js
- One route file per resource in routes/ folder
- Data persistence handled via db/store.js file-based store
