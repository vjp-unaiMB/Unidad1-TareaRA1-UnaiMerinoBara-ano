# Comparación estructural de los lenguajes más famosos.

En este apartado compararé algunos de los **lenguajes** de programación **más populares** y con distintos usos. Poniendo en valor la **seguridad** que ofrecen mediante un breve resumen.


|Lenguaje	|Tipo de Ejecución|	Paradigma Principal	|Seguridad de Memoria ​|
|---|---|---|---|
|Java	|Compilado  ​	|Orientado a Objetos ​	|Alta gracias a sandbox y garbage collector que aíslan código y evitan fugas de memoria.|
|Python	|Interpretado ​	|Multiparadigma ​	|Media con tipado dinámico permite inyecciones si no se valida input correctamente.|
|Rust	|Compilado  ​	|Multiparadigma (sistemas) ​	|Muy alta por ownership y borrow checker que previene race conditions y accesos inválidos.|
|C++	|Compilado  ​	|Multiparadigma  ​	|Baja por su gestión manual propensa a buffer overflows y errores de punteros.
|PHP	|Interpretado ​	|Multiparadigma (web, procedural) ​	|Media-baja, es vulnerable a SQL injection sin sanitización adecuada de entradas.
|JavaScript	|Interpretado (JIT en navegadores) ​	|Multiparadigma​	|Media,gracias a que el sandbox del navegador mitiga, pero el XSS es común por manipulación DOM insegura.
|C	|Compilado nativo ​	|Procedural/imperativo ​	|Muy baja, los punteros y memoria manual causan exploits frecuentes como overflows.

## Conclusiones

* Considero que de base los lenguajes compilados son más seguros que los interpretados, debido a que cuando debes de compilar el código, dicho proceso de traducción obliga al usuario a implementar el código correctamente antes de su ejecución, lo que asegura que la estructura del código es óptima.

* Los 2 lenguajes más seguros de la tabla son Java y Rust, sus estructuras son las más sólidas, asegurando que cualquier error sea controlable, por otra parte C y C++ requieren disciplina extrema debido a su bajo nivel. 

* Rust es el más sólido y seguro de todos al ser compilado y ser de bajo nivel, no obstante tiene abstracciones seguras que lo acercan a características de alto nivel, lo que lo hace más fácil de trabajar y familiar que el resto de lenguajes de su categoría.