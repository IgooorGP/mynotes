# CORS

## Browser requests

Browsers use a mechanism called **pre-flight** which makes a request to the backend in order to verify the CORS policy defined by the backend:

    Browser ------- pre-flight -------> [backend]
    Browser <------ CORS policy ------- [backend]

The browser will then check if the following are allowed by the backend server:

* HTTP verb;
* Headers used on the request;
* Origin of the request = protocal + host + port (e.g.: http://localhost)

CORS is validated by the browser!

## Servers Running on different ports

If the frontend server is running on a given port (localhost:4200) and  the backedn is running on ANOTHER port (localhost:8000), we then have CORS requests:

    Browser --> localhost:4200 (front) --CORS--> localhost:8000 (backend)


## Headers

```Access-Control-Allow-Origin: <origin>```: Indicates if the contents of the response can or cannot be shared with the **given** origin.

Hence, the backend server can declare specific origins which the response will be served or not!

    Browser (origin1) <------ NO sharing with you  ------- [backend]

    Browser (origin2) <---------------- OK! -------------- [backend]

```Access-Control-Request-Headers: <header1>, <header2> ...```: Used by the pre-flight mechanism, which allows the backend to know which HTTP headers will be sent in the real request.

    Browser ------ hey, I'll use these headers --------> [backend]

```Access-Control-Allow-Headers <allowedHeader1> ...```: Used on the response of the backend server to indicate WHICH HTTP headers are allowed in the real request.

    Browser <------ actually, you can use these headers -------- [backend]