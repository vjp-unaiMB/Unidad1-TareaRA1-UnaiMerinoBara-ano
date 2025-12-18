# Elementos de Python

Dentro de este apartado explicaremos el funcionamiento del programa ***Lavadero*** analizando las estructuras que lo componen.

## Características importantes del programa.

**Ideas Claras**

* La primera característica y la más importante es que este programa ha sido elaborado en el lenguaje de programación Python

* Emplea el paradigma de programación orientado a objetos. Este paradigma de programación consiste en la creación de objetos mediante clases. Dentro de estas clases, se estructuran los atributos y características que queremos que contengan nuestros objetos a la hora de ser creados, lo que nos lleva a analizar su estructuración.

* La estructura de este programa se compone de dos archivos, 
    1. El primero de ellos se llama main, es el fichero principal y donde compienza la ejecución del programa, por lo que deducimos que es donde se van a crear los objetos y otros bloques de código que interactuarán con los objetos.

    2. El segundo fichero es la clase ***lavadero***. Dentro de esta clase se declaran las constantes y variables que conformarán las características de las que dispondrá el objeto ***lavadero***. La clase también está conformada por otras estructuras que se encargan de que dichas variables sean manipulables. Aquí es donde entran:

        * **Constructores:** Estas estructuras se encargan de agrupar los parámetros introducidos durante la creación de un bobjeto ***lavadero*** y aplicar cada valor a la propiedad correspondiente del objeto.

       *  **@property's**  Estos elementos se encargan de devolver el valor de un atribuo de un objeto en caso de llamada.

       *  **Métodos internos** Nos servirán para realizar algoridmos internos y automatizar características y comportamientos del objeto en base a nuestros parámetros.

## Análisis del código

Desglosaremos cada uno de los archivos que conforman ***Lavadero***.

### Main.py

Encabezamos el fichero importando la clase lavadero.

```
from lavadero import Lavadero
```

Este método encarga de simular el proceso de lavado para un vehículo con las opciones dadas introduciendo los siguientes parámetros:
* `lavadero`: Instancia de Lavadero.
* `prelavado`: bool, True si se solicita prelavado a mano.
* `secado_mano`: bool, True si se solicita secado a mano.
* `encerado`: bool, True si se solicita encerado.
  
Tras ello, pinta por pantalla las opciones introducidas y anuncia el inicio de la simulación. También captura excepciones mediante la estructura `Try`

Al comenzar la simulación pasa el valor de las fases a 0 Imprime el estado de cada prueba y avanza fase por fase hasta llegar al final, imprimiendo la información en cada parte.

```
def ejecutarSimulacion(lavadero, prelavado, secado_mano, encerado):
    
    print("--- INICIO: Prueba de Lavado con Opciones Personalizadas ---")
    print(f"Opciones solicitadas: [Prelavado: {prelavado}, Secado a mano: {secado_mano}, Encerado: {encerado}]")

    # 1. Iniciar el lavado
    try:
        lavadero.hacerLavado(prelavado, secado_mano, encerado)
        lavadero.imprimir_estado()

        print("\nAVANZANDO FASE POR FASE:")

        pasos = 0
        while lavadero.ocupado and pasos < 20: 
            # El cobro ahora ocurre en la primera llamada a avanzarFase (transición 0 -> 1)
            lavadero.avanzarFase()
            print(f"-> Fase actual: ", end="")
            lavadero.imprimir_fase()
            print() 
            pasos += 1
        
        print("\n----------------------------------------")
        print("Lavado completo. Estado final:")
        lavadero.imprimir_estado()
        print(f"Ingresos acumulados: {lavadero.ingresos:.2f} €")
        print("----------------------------------------")
        
    except ValueError as e: # Captura la excepción de regla de negocio (Requisito 2)
        print(f"ERROR DE ARGUMENTO: {e}")
    except RuntimeError as e: # Captura la excepción de estado (Requisito 3)
        print(f"ERROR DE ESTADO: {e}")
    except Exception as e:
        print(f"ERROR INESPERADO: {e}")
```

En este punto del código, tras los métodos vistos, encontramos el lugar desde donde empezará a ejecutarse el código. Tras este comentario, si el fichero es main, creará una instancia del lavadero sin concretar ningún parámetro. Tras ello, se llamará al método `ejecutarSimulacion()` aquí será donde introduciremos por parámetro la instanca y sus características principales. Según lo que introduzcamos, `ejecutarSimulacion()` añadirá los valores añadidos de cada complemento, dejando un formato resultante que nos permitirá comparar cada prueba. Disponiendo de 4 ejemplos. 

En este caso nos encontramos con el **EJEMPLO 1:**

```
# Punto de entrada (main): Aquí pasamos los parámetros
if __name__ == "__main__":
    
    lavadero_global = Lavadero() 
    
    # EJEMPLO 1: Lavado completo con prelavado, secado a mano, con encerado (Requisito 8 y 14)
    # Precio esperado: 5.00 + 1.50 + 1.00 + 1.20 = 8.70 €
    print("\n=======================================================")
    print("EJEMPLO 1: Prelavado (S), Secado a mano (S), Encerado (S)")
    ejecutarSimulacion(lavadero_global, prelavado=True, secado_mano=True, encerado=True)
```

**EJEMPLO 2:** Lavado rápido sin extras (Precio esperado: 5.00 €)


```
    print("\n=======================================================")
    print("EJEMPLO 2: Sin extras (Prelavado: N, Secado a mano: N, Encerado: N)")
    ejecutarSimulacion(lavadero_global, prelavado=False, secado_mano=False, encerado=False)
```

**EJEMPLO 3:** Lavado con encerado, pero sin secado a mano (Debe lanzar ValueError - Requisito 2)

```
    print("\n=======================================================")
    print("EJEMPLO 3: ERROR (Encerado S, Secado a mano N)")
    ejecutarSimulacion(lavadero_global, prelavado=False, secado_mano=False, encerado=True)
```
**EJEMPLO 4:** Lavado con prelavado a mano (Requisito 4 y 10) Precio esperado: 5.00 + 1.50 = 6.50 €


```
    print("\n=======================================================")
    print("EJEMPLO 4: Prelavado (S), Secado a mano (N), Encerado (N)")
    ejecutarSimulacion(lavadero_global, prelavado=True, secado_mano=False)
```


### Lavadero.py

Como comenté anteriormente, esta fichero es donde se declara la estructura de un objeto, como si fuera un molde. Como nuestro programa emula el funcionamiento de un ***lavadero***, esta clase es indispensable para poder crear dicho objeto. 

Vamos a analiar la clase ***lavadero***.

```python
# lavadero.py

class Lavadero:

    FASE_INACTIVO = 0
    FASE_COBRANDO = 1
    FASE_PRELAVADO_MANO = 2
    FASE_ECHANDO_AGUA = 3
    FASE_ENJABONANDO = 4
    FASE_RODILLOS = 5
    FASE_SECADO_AUTOMATICO = 6
    FASE_SECADO_MANO = 7
    FASE_ENCERADO = 8
    
```

Justo debajo contamos con un un constructor de la clase un constructor no es más que una estructura que se encarga de inicializar el **objeto** lavadero, añadiendo una serie de valores predeterminados a sus **variables**. A demás, también puede invocar métodos, como es el caso de la última sentencia `self.terminar()`


```python
def __init__(self):
        self.__ingresos = 0.0
        self.__fase = self.FASE_INACTIVO
        self.__ocupado = False
        self.__prelavado_a_mano = False
        self.__secado_a_mano = False
        self.__encerado = False
        self.terminar() 
        
```

Las estructuras que contienen el decorador @property son convertidas en un **método** de solo lectura, permitiendo acceder a atributos privados de forma directa mediante objeto. Cada uno de estos métodos, nos devolverá el valor de cada propiedad del objeto al ser invocada.

En este caso:

* Fase
* Ingresos (Numérico)
* Ocupado (Booleano)
* prelavado_a_mano (Booleano)
* Secado_a_mano (Booleano)
* Encerado (Booleano)
       


```python

    @property
    def fase(self):
        return self.__fase

    @property
    def ingresos(self):
        return self.__ingresos

    @property
    def ocupado(self):
        return self.__ocupado
    
    @property
    def prelavado_a_mano(self):
        return self.__prelavado_a_mano

    @property
    def secado_a_mano(self):
        return self.__secado_a_mano

    @property
    def encerado(self):
        return self.__encerado
```

Este **méodo terminar** se encarga de poner los valores de las variables del objeto (fase,ocupado,prelavado_a_mano,secado_a_mano y encerado) a *false*.


```python

    def terminar(self):
        self.__fase = self.FASE_INACTIVO
        self.__ocupado = False
        self.__prelavado_a_mano = False
        self.__secado_a_mano = False
        self.__encerado = False
        
```

Este **méodo hacerLavado** se encarga de dar feedback al usuario sobre el lavadero. Controla los valores de las variables del objeto (ocupado,secado_a_mano y encerado mediante una estructura de if e if not, de tamlanera que si el lavadero está ocupado, le advertirá al usuario que no puede ocuparlo haste que se libere. Si se puede entrar, cambiará de valor las variables del lavadero a las indicadas y avisará sobre el encerado manual.


```python

    
    def hacerLavado(self, prelavado_a_mano, secado_a_mano, encerado):
        if self.__ocupado:
            raise RuntimeError("No se puede iniciar un nuevo lavado mientras el lavadero está ocupado")
        
        if not secado_a_mano and encerado:
            raise ValueError("No se puede encerar el coche sin secado a mano")
        
        self.__fase = self.FASE_INACTIVO  
        self.__ocupado = True
        self.__prelavado_a_mano = prelavado_a_mano
        self.__secado_a_mano = secado_a_mano
        self.__encerado = encerado
        
```

Este **méodo cobrar** se encarga de calcular y añadir los ingresos según las opciones seleccionadas de lavado.
Se fija un precio base: 5.00€ y va añadiendo un incremento según los extras añadidos, se controla mediane varias estructuras IF. Finalmente, retorna el valor del ingreso total con el **Return**.


```python

    def _cobrar(self):
        """
         (Implícito, 5.00€ de base + 1.50€ de prelavado + 1.00€ de secado + 1.20€ de encerado = 8.70€)
        """
        coste_lavado = 5.00
        
        if self.__prelavado_a_mano:
            coste_lavado += 1.50 
        
        if self.__secado_a_mano:
            coste_lavado += 1.20 
            
        if self.__encerado:
            coste_lavado += 1.00 
            
        self.__ingresos += coste_lavado
        return coste_lavado
```

Este **método avanzarFase** nos permite cambiar la fase según la fase en la que nos encontramos, está compuesto por una estructura de ifs que controlan la dase en la que se encuentra el usuario y según en la que se encuentre realiza una función distinta. En algunas de estas fases, ejecuta invocaciones a otros métodos y reproduce mensajes.


```python
def avanzarFase(self):
       
        if not self.__ocupado:
            return

        if self.__fase == self.FASE_INACTIVO:
            coste_cobrado = self._cobrar()
            self.__fase = self.FASE_COBRANDO
            print(f" (COBRADO: {coste_cobrado:.2f} €) ", end="")

        elif self.__fase == self.FASE_COBRANDO:
            if self.__prelavado_a_mano:
                self.__fase = self.FASE_PRELAVADO_MANO
            else:
                self.__fase = self.FASE_ECHANDO_AGUA 
        
        elif self.__fase == self.FASE_PRELAVADO_MANO:
            self.__fase = self.FASE_ECHANDO_AGUA
        
        elif self.__fase == self.FASE_ECHANDO_AGUA:
            self.__fase = self.FASE_ENJABONANDO

        elif self.__fase == self.FASE_ENJABONANDO:
            self.__fase = self.FASE_RODILLOS
        
        elif self.__fase == self.FASE_RODILLOS:
            if self.__secado_a_mano:
                self.__fase = self.FASE_SECADO_AUTOMATICO 

            else:
                self.__fase = self.FASE_SECADO_MANO
        
        elif self.__fase == self.FASE_SECADO_AUTOMATICO:
            self.terminar()
        
        elif self.__fase == self.FASE_SECADO_MANO:

            self.terminar() 
        
        elif self.__fase == self.FASE_ENCERADO:
            self.terminar() 
        
        else:
            raise RuntimeError(f"Estado no válido: Fase {self.__fase}. El lavadero va a estallar...")

```

Este **método imprimir fase** Se invoca junto a un parámetro numérico. Genera un mapa de frases que controlaremos mediante el parámetro introducido, al final del método se hace un print en el que se invoca el mala de frases pasando directamente el parámetro.


```python

    def imprimir_fase(self):
        fases_map = {
            self.FASE_INACTIVO: "0 - Inactivo",
            self.FASE_COBRANDO: "1 - Cobrando",
            self.FASE_PRELAVADO_MANO: "2 - Haciendo prelavado a mano",
            self.FASE_ECHANDO_AGUA: "3 - Echándole agua",
            self.FASE_ENJABONANDO: "4 - Enjabonando",
            self.FASE_RODILLOS: "5 - Pasando rodillos",
            self.FASE_SECADO_AUTOMATICO: "6 - Haciendo secado automático",
            self.FASE_SECADO_MANO: "7 - Haciendo secado a mano",
            self.FASE_ENCERADO: "8 - Encerando a mano",
        }
        print(fases_map.get(self.__fase, f"{self.__fase} - En estado no válido"), end="")
        
    
```

Este **método imprimir_estado** Se invoca junto a un parámetro numérico. Imprime una serie de sentencias en las que se informa del estado de lavado al usuario.


```python
def imprimir_estado(self):
        print("----------------------------------------")
        print(f"Ingresos Acumulados: {self.ingresos:.2f} €")
        print(f"Ocupado: {self.ocupado}")
        print(f"Prelavado a mano: {self.prelavado_a_mano}")
        print(f"Secado a mano: {self.secado_a_mano}")
        print(f"Encerado: {self.encerado}")
        print("Fase: ", end="")
        self.imprimir_fase()
        print("\n----------------------------------------")
        
```

Este **método ejecutar_y_obtener_fases** Se invoca junto a un parámetro numérico y 3 parámetros adicionales referentes a prelavado, secado y encerado. Imprime una serie de sentencias en las que se informa del estado de lavado al usuario. Ejecutando un ciclo completo y devolviendo un array de en lista de fases visitadas.



```python

    def ejecutar_y_obtener_fases(self, prelavado, secado, encerado):
        self.lavadero.hacerLavado(prelavado, secado, encerado)
        fases_visitadas = [self.lavadero.fase]
        
        while self.lavadero.ocupado:
            # Usamos un límite de pasos para evitar bucles infinitos en caso de error
            if len(fases_visitadas) > 15:
                raise Exception("Bucle infinito detectado en la simulación de fases.")
            self.lavadero.avanzarFase()
            fases_visitadas.append(self.lavadero.fase)
            
        return fases_visitadas

```


