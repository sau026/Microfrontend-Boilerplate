🚀 Micro Frontend with Module Federation
A production-ready micro frontend architecture using Webpack 5 Module Federation, demonstrating how to build independently deployable frontend applications that work together seamlessly.
<img width="1329" height="678" alt="image" src="https://github.com/user-attachments/assets/c5ca0c61-2db0-491c-991d-f40c93d46b33" />

📋 Table of Contents

Overview
Architecture
Features
Prerequisites
Installation
Running the Project
Project Structure
How It Works
Key Concepts
Configuration
Development
Building for Production
Troubleshooting
Contributing


🎯 Overview
This project demonstrates a Micro Frontend Architecture using Webpack 5 Module Federation. It consists of three independent applications:

Shell App (Host) - Main container application using CRACO
Header App (Remote) - Navigation component using pure Webpack
Products App (Remote) - Product listing component using pure Webpack

Each application can be developed, tested, and deployed independently while working together seamlessly in the shell application.
🏗️ Architecture
<img width="417" height="285" alt="image" src="https://github.com/user-attachments/assets/d1119b46-c3ee-4ffb-9a9f-027adc998890" />


        Shared Dependencies (React, React-DOM, etc.)
✨ Features

🏗️ Independent Development - Each micro frontend can be developed separately
🚀 Independent Deployment - Deploy micro frontends without affecting others
🔄 Shared Dependencies - Efficient sharing of React and libraries
📦 Module Federation - Webpack 5's built-in micro frontend support
🎨 Modern UI - Beautiful, responsive design with CSS animations
🛡️ Error Boundaries - Graceful fallbacks when micro frontends fail
🔥 Hot Module Replacement - Fast development experience
📱 Mobile Responsive - Works perfectly on all screen sizes

📦 Prerequisites
Before you begin, ensure you have the following installed:

Node.js (v16 or higher)
npm (v7 or higher) or yarn
Git

Check your versions:
bashnode --version
npm --version
🚀 Installation
1. Clone the Repository
bashgit clone https://github.com/yourusername/micro-frontend-project.git
cd micro-frontend-project
2. Install Dependencies for Each App
bash# Install Shell App dependencies
cd shell-app
npm install

# Install Header App dependencies
cd ../header-app
npm install

# Install Products App dependencies
cd ../products-app
npm install
🏃 Running the Project
You need to run all three applications simultaneously. Open three separate terminal windows:
Terminal 1: Shell App (Host)
bashcd shell-app
npm start
Runs on: http://localhost:3000
Terminal 2: Header App (Remote)
bashcd header-app
npm start
Runs on: http://localhost:3001
Terminal 3: Products App (Remote)
bashcd products-app
npm start
Runs on: http://localhost:3002
🎉 Access the Application
Once all three apps are running, visit:

Main Application: http://localhost:3000
Header Standalone: http://localhost:3001
Products Standalone: http://localhost:3002

📁 Project Structure
<img width="402" height="543" alt="image" src="https://github.com/user-attachments/assets/b66340ac-fbc3-479e-9331-9d6bd61fde29" />



🔍 How It Works
Module Federation Configuration
Shell App (Consumer/Host)
javascript// craco.config.js
new ModuleFederationPlugin({
  name: "shell",
  remotes: {
    header: "header@http://localhost:3001/remoteEntry.js",
    products: "products@http://localhost:3002/remoteEntry.js"
  },
  shared: {
    react: { singleton: true, eager: true },
    "react-dom": { singleton: true, eager: true }
  }
})
Header App (Provider/Remote)
javascript// webpack.config.js
new ModuleFederationPlugin({
  name: "header",
  filename: "remoteEntry.js",
  exposes: {
    "./Header": "./src/components/Header"
  },
  shared: {
    react: { singleton: true, eager: true },
    "react-dom": { singleton: true, eager: true }
  }
})
Component Usage
javascript// In Shell App
const Header = React.lazy(() => import("header/Header"));
const Products = React.lazy(() => import("products/ProductList"));

// Render with Suspense
<Suspense fallback={<Loading />}>
  <Header />
</Suspense>
🎓 Key Concepts
1. Module Federation
Webpack 5's built-in feature for sharing code between applications at runtime.
2. Shared Dependencies
Common libraries (like React) are loaded once and shared across all micro frontends:
javascriptshared: {
  react: { singleton: true }  // Only one React instance
}
3. Remote & Host

Host (Shell): Consumes components from remotes
Remote (Header, Products): Exposes components to be consumed

4. Bootstrap Pattern
Prevents "shared module is not available for eager consumption" error:
javascript// index.js
import("./bootstrap");  // Dynamic import for async loading
5. Error Boundaries
Graceful fallbacks when micro frontends fail to load:
javascript<ErrorBoundary fallback={<div>Component unavailable</div>}>
  <RemoteComponent />
</ErrorBoundary>
⚙️ Configuration
Port Configuration
Default ports:

Shell App: 3000
Header App: 3001
Products App: 3002

To change ports, update:

craco.config.js (shell-app)
webpack.config.js (header-app, products-app)
Remote URLs in shell app configuration

Shared Dependencies
Add new shared dependencies in all app configurations:
javascriptshared: {
  react: { singleton: true, eager: true },
  "react-dom": { singleton: true, eager: true },
  "your-library": { singleton: true }  // Add here
}
🛠️ Development
Adding a New Micro Frontend

Create new React app or Webpack project
Configure Module Federation:

javascriptnew ModuleFederationPlugin({
  name: "newApp",
  filename: "remoteEntry.js",
  exposes: {
    "./Component": "./src/Component"
  },
  shared: { /* ... */ }
})

Add remote to shell app:

javascriptremotes: {
  newApp: "newApp@http://localhost:3003/remoteEntry.js"
}
Development Tips

Keep ports consistent across environments
Test standalone mode for each micro frontend
Use Error Boundaries for all remote components
Monitor bundle sizes to optimize shared dependencies
Version control shared dependency versions

🏗️ Building for Production
Build All Apps
bash# Build Shell App
cd shell-app
npm run build

# Build Header App
cd ../header-app
npm run build

# Build Products App
cd ../products-app
npm run build
Production Configuration
Update remote URLs to production domains:
javascriptremotes: {
  header: "header@https://header.yourdomain.com/remoteEntry.js",
  products: "products@https://products.yourdomain.com/remoteEntry.js"
}
Deployment Strategies

Independent Deployment: Deploy each micro frontend separately
CDN Distribution: Host remoteEntry.js on CDN
Version Management: Use semantic versioning for compatibility
Canary Releases: Deploy to subset of users first

🐛 Troubleshooting
Common Issues
1. "Shared module is not available for eager consumption"
Solution: Ensure bootstrap pattern is used in all apps
javascript// index.js
import("./bootstrap");
2. Remote component not loading
Solution:

Check if remote app is running
Verify remote URL is correct
Check browser console for errors
Ensure CORS is properly configured

3. Version conflicts
Solution: Align React versions across all apps
bashnpm list react  # Check installed version
4. Module not found errors
Solution: Install missing dependencies
bashnpm install missing-package
5. Port already in use
Solution: Kill process or change port
bash# Find process on port 3000
lsof -ti:3000

# Kill process
kill -9 <PID>
Debug Mode
Enable verbose webpack output:
javascript// webpack.config.js
module.exports = {
  stats: 'verbose'
}
📊 Performance Optimization
Bundle Analysis
Analyze bundle sizes:
bashnpm install -D webpack-bundle-analyzer

# Add to webpack config
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

plugins: [
  new BundleAnalyzerPlugin()
]
Optimization Tips

Lazy Loading: Use React.lazy for code splitting
Shared Dependencies: Maximize shared libraries
CDN Caching: Host static assets on CDN
Compression: Enable gzip/brotli compression
Tree Shaking: Remove unused code

🧪 Testing
Unit Tests
bashnpm test
E2E Tests
Consider using Cypress or Playwright for end-to-end testing across micro frontends.
🤝 Contributing
Contributions are welcome! Please follow these steps:

Fork the repository
Create a feature branch (git checkout -b feature/amazing-feature)
Commit your changes (git commit -m 'Add amazing feature')
Push to the branch (git push origin feature/amazing-feature)
Open a Pull Request

Coding Standards

Use ESLint and Prettier
Write meaningful commit messages
Add tests for new features
Update documentation

📚 Additional Resources

Webpack Module Federation Documentation
Micro Frontends by Martin Fowler
Module Federation Examples

🎓 Learning Path

✅ Understand Module Federation basics
✅ Set up simple host + remote
✅ Add shared dependencies
✅ Implement error handling
⬜ Add authentication across apps
⬜ Implement shared state management
⬜ Deploy to production

👥 Authors

Your Name - Saurabh Kumar - sau026

🙏 Acknowledgments

Webpack team for Module Federation
React team for amazing framework
Community for best practices and examples


⭐ Show Your Support
Give a ⭐️ if this project helped you learn micro frontends!

Built with ❤️ using React and Webpack Module Federation
