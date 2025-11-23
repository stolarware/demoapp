Go HTMX Application
This project is a simple web application built using Go (Golang) and HTMX. It demonstrates how to create a dynamic web page that can load content without a full page refresh.

Project Structure
------------------
go-htmx-app
├── src
│   ├── main.go          # Entry point of the application
│   └── handlers
│       └── home.go     # Handler for the home route
├── web
│   ├── static
│   │   └── htmx.min.js  # Minified HTMX library
│   └── templates
│       └── index.html   # HTML template for the application
├── go.mod               # Module definition
└── README.md            # Project documentation
Setup Instructions
Clone the repository:

git clone <repository-url>
cd go-htmx-app
Install dependencies: Ensure you have Go installed on your machine. Run the following command to download the necessary dependencies:

go mod tidy
Run the application: Start the server by executing:

go run src/main.go
Access the application: Open your web browser and navigate to http://localhost:8080 to view the application.

Usage
The application serves a simple HTML page that utilizes HTMX for dynamic content loading. You can modify the index.html file to add more interactive elements as needed.
