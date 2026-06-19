2do Parcial - Parte Practica
Que se solicita:

El codigo tiene 10 errores. Recae en usted analizar que es un error dentro del codigo.
Los Alumnos tendran que forkear este repo como propio, hacer un issue desde Github con Comentarios refiriendo en que linea esta el error, y como se debe solucionar.
La respuesta sera con el link a ese Fork, y adentro deben estar los issues. Los profesores tenemos que poder ingresar al mismo. Recae en los alumnos asegurarse de que los profesores puedan ingresar.
Tambien pueden editar el Archivo Readme y poner los resultados dentro de sus propios forks.
https://github.com/ExBattou/SimpsonsApp


1- Error al compilar
Gradle property is invalid (Java home supplied is invalid)

El error se da en el file gradle.properties debido a que esta el JDK 17. Eliminar esta linea permite bajar las dependencias con Gradle de forma correcta.

2- Error en la estructura del proyecto.
Por ejemplo: Existe un package main, el cual tiene mezclados screens y viewModels. Esto no debería ocurrir. Cada componente deberia ir en su package correspondiente. los screens en UI y los viewModel en un package logic.viewModel

3- En el file Episode.kt hay una clase sin declarar de forma correcta, por lo que al realizar un build arroja el siguiente error:
file:///C:/examen_desaApps/SimpsonsApp/app/src/main/java/com/example/simpsonsapp/domain/model/Episode.kt:13:1 Expecting a top level declaration
Se resuelve declarando la clase de forma correcta indicando que va a retornar

4- Existe un error en tiempo de compilación (KAPT), el cual intenta cargar SQLite nativo para verificar mis schema.
En mi entorno Windows y JDK 17, no encuentra el binario nativo de sqlite-jdbc en el classpath del procesador.
Esto se resuelve agregando la dependencia de sqlite-jdbc en el file build.gradle.kts. 
  kapt("org.xerial:sqlite-jdbc:3.53.2.0")

5- En los Dto existen muchos atributos no nulleables como por ejemplo en EpisodesResponse.kt, EpisodeDto tiene muchos String/Int no-null.
Esto provoca riesgo de crash o fallos de parseo si se quieren mapear datos que devuelve null. 

6- Error detectado: La data class Episode no tiene el componente @Entity y se pretende utilizar como entidad para realizar llamadas a la base de datos con el @Dao.

7- Mala practica de endpoint absoluto en @GET
@GET("https://thesimpsonsapi.com/api/episodes")
Rompiendo la estrategia de baseUrl de Retrofit, dificulta ambientes (dev/stage/prod), reutilización y testing.
En lugar de esto deberiamos usar @GET("api/episodes") y centralizar host en provider de Retrofit.
