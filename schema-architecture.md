Sección 1: Resumen de la arquitectura

Esta aplicación de Spring Boot utiliza tanto controladores MVC como REST. Se utilizan plantillas de Thymeleaf para los paneles de administración y de doctor, mientras que las API REST sirven a todos los demás módulos. La aplicación interactúa con dos bases de datos: MySQL (para datos de pacientes, doctores, citas y administración) y MongoDB (para recetas). Todos los controladores dirigen las solicitudes a través de una capa de servicio común, que a su vez delega en los repositorios apropiados. MySQL utiliza entidades JPA mientras que MongoDB utiliza modelos de documentos.

Sección 2: Flujo numerado de datos y control

1. Los usuarios pueden acceder a la aplicación a través de: Tableros web basados en Thymeleaf como AdminDashboard y DoctorDashboard. Clientes de API REST ( Appointments, PatientDashboard y PatientRecord) interactúan con el backend a través de HTTP y reciben respuestas en JSON.

2. Cuando un usuario interactúa con la aplicación  la solicitud se dirige a  Controladores de Thymeleaf en el backend según la ruta de la URL y el método HTTP. Estas devuelven plantillas .html que se llenarán con datos dinámicos y se renderizarán en el navegador.
   Las solicitudes de los consumidores de API son manejadas por Controladores REST que devuelven respuestas en formato JSON.


3. A traves de servicios se coordina los flujos de trabajo entre las entidades 


4. Los servicio se comunican con los Repositorios para el acceso a datos. El Repositorio MySQL, que utilizan Spring Data JPA para gestionar datos relacionales estructurados como pacientes, doctores, citas y registros administrativos y el Repositorio MongoDB, que utiliza Spring Data MongoDB para gestionar registros basados en documentos como recetas.


5. Cada uno de los repositorio se conectan directamente con el motor de base de datos.

6. Cuando se recuperan los datos de la base de datos, se mapean en clases de modelo de Java con las que la aplicación puede trabajar. En el caso de MySQL, los datos se convierten en entidades JPA, que representan filas en tablas relacionales y están anotadas con @Entity.
Para MongoDB, los datos se cargan en objetos de documento, típicamente anotados con @Document, que se mapean a estructuras BSON/JSON en colecciones.


7. Finalmente, los modelos vinculados envian respuestas, en flujos MVC, los modelos se pasan del controlador a las plantillas de Thymeleaf, donde se renderizan como HTML dinámico para el navegador. En flujos REST, los mismos modelos (o DTOs transformados) se serializan en JSON y se envían de vuelta al cliente como parte de una respuesta HTTP.
