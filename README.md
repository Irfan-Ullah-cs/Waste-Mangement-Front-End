# Waste Management System - Frontend

The **Waste Management System** is a comprehensive project designed to optimize waste collection and monitoring. It integrates multiple technologies to provide real-time updates, driver navigation, and IoT-based bin monitoring. The system consists of the following components:

- **Frontend**: Developed with Vue.js for an intuitive user interface.
- **Backend**: Built using Spring Boot to manage business logic and APIs.
- **Mobile Application**: Created with Kotlin using Jetpack Compose for Android devices.
- **IoT Integration**: Real-time monitoring of bin statuses using IoT devices.

---

## Project Overview

This system is designed to efficiently monitor and manage waste collection. Below is an overview of its core functionalities:

1. **IoT Integration**:
   - IoT devices monitor the status of bins (e.g., full or empty) in real time.
   - Updates are sent to the backend.

2. **Backend Functionality**:
   - The backend (Spring Boot) serves as the central hub, handling data from IoT devices.
   - It ensures real-time updates are reflected on the frontend and mobile app.

3. **Driver Dashboard**:
   - Drivers can access the bin status through the mobile app.
   - Features include map functionality to locate bins and navigate routes.
   - Drivers can collect waste from full bins as directed by the system.

4. **Additional Features**:
   - Advanced monitoring and route optimization for drivers.
   - Centralized communication between IoT devices, the backend, and the frontend/mobile app.

---

## Requirements

To run the **frontend**, ensure you have the following installed:

- **Node.js** (v16 or later)
- **Yarn** (v1.22 or later)

For the entire project, you will also need:

- **Backend**: Spring Boot (Java 11 or later)
- **Mobile Application**: Android Studio (for running the Kotlin app)
- **IoT Devices**: Configured to send real-time data to the backend.

---


## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```


## Features Summary

- **Real-Time Monitoring**: Bin statuses are updated instantly through IoT devices.
- **Driver Navigation**: Drivers can view bin locations and navigate using map functionality.
- **Integrated System**: Seamless communication between IoT devices, backend, and frontend/mobile app.
- **Efficient Waste Collection**: Drivers are notified of full bins and can plan optimized routes.

---

## Contribution Guidelines

We welcome contributions! To contribute:

1. Fork the repository.
2. Create a feature branch.
3. Submit a pull request with your changes.

---

## License

This project is licensed under the [MIT License](LICENSE).

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
