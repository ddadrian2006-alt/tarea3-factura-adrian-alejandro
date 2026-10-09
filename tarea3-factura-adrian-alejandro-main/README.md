# Tarea 3 · La factura bajo el microscopio
Pareja: Adrián Durán y Alejandro Sánchez · GitHub: ddadrian2006-alt, alejandro-sánchez0017

## Reto 1 · Compilar desde la terminal
- **Comando usado:** `javac Factura.java`
- **Resultado:** Error de compilación en la terminal. No se ha generado el archivo binario (captura: `capturas/reto1.png`).
- **¿Qué es Factura.java?** Es el archivo con el código fuente en texto plano escrito por el programador.
- **¿Qué es Factura.class?** Es el archivo ejecutable resultante que contiene el *bytecode*, un código intermedio ejecutable por la máquina virtual.
- **¿Quién ejecuta el .class?** La Máquina Virtual de Java (JVM - *Java Virtual Machine*).
- **¿Por qué no se ha generado Factura.class la primera vez?** Porque el código fuente `Factura.java` contiene errores de compilación y el compilador (`javac`) detiene el proceso antes de generar el *bytecode*.

---

## Reto 2 · Errores de compilación
| Nº | Línea | Mensaje de javac | Tipo (léxico / sintaxis / semántica) | Corrección |
|----|-------|------------------|--------------------------------------|------------|
| 1  | 14    | `not a statement` / carácter ilegal `#` | Léxico | Eliminar el carácter `#` en el nombre de la variable: cambiar `precio#` por `precio`. |
| 2  | 16    | `';' expected` | Sintaxis | Añadir el punto y coma `;` al final de la instrucción `int cantidad = sc.nextInt()`. |
| 3  | 27    | `incompatible types: possible lossy conversion from double to int` | Semántica | Cambiar el tipo de dato de la variable `total` de `int` a `double`. |

Captura del programa funcionando: `capturas/reto2.png`

> **Cálculo manual previo al Reto 3:**  
> Para precio = 60, cantidad = 2 (base = 120 €) y socio = "s":  
> - Base > 100 € → 10% de descuento = 12 €  
> - Socio → 5% adicional = 6 €  
> - Descuento total = 18 €  
> - Importe con descuento = 120 - 18 = 102 €  
> - Total con 21% IVA = 102 × 1.21 = **123.42 €** (El programa devolvía erróneamente 137.94 €).

---

## Reto 3 · Depuración del error lógico
| Variable | Valor esperado | Valor real |
|----------|----------------|------------|
| descuento | 18.00 € | 6.00 € |
| total     | 123.42 € | 137.94 € |

- **Línea del error y explicación:** Línea 24 (`descuento = base * 0.05;`). Se utiliza una asignación simple `=` en lugar de una acumulación `+=`. Al evaluar la condición de cliente socio, el segundo `if` sobrescribe el descuento previo del 10% aplicado en la línea 21 por el del 5%, en lugar de sumarlo.
- **Corrección:** Cambiar la instrucción a `descuento += base * 0.05;` (o calcular el porcentaje acumulado del 15%).
- **Mi propia prueba:**
  - **Producto:** Monitores
  - **Precio unitario:** 150.00 €
  - **Cantidad:** 2
  - **Socio:** s
  - **Resultado:** Base = 300.00 €, Descuento (15%) = 45.00 €, Total con IVA (255 × 1.21) = **308.55 €**

Captura del depurador: `capturas/reto3.png`

---

## Reto 4 · Reutilización
- **Métodos creados y para qué sirve cada uno:**
  - `static double calcularDescuento(double base, boolean socio)`: Calcula el descuento aplicando un 10% si la base supera los 100 € y suma un 5% adicional si el cliente es socio.
  - `static double aplicarIVA(double importe)`: Recibe el importe neto tras el descuento y le aplica el 21% de IVA.
- **¿Qué ventaja tiene frente al código anterior?**  
  Mejora la modularidad, la legibilidad y la mantenibilidad del software. Permite reutilizar la lógica de cálculo financiero en otras partes de la aplicación sin duplicar código y facilita la realización de pruebas unitarias independientes sobre cada método.

---

## Reto 5 · ¿Qué lenguaje elegirías?
- **Caso A (web de la tienda):**  
  **Tipo de lenguaje:** Interpretado / Scripting (JavaScript / TypeScript).  
  **Justificación:**  
  1. *Compatibilidad e integración en cliente:* Ejecución nativa directa dentro de los navegadores web sin requerir instalaciones previas por parte del usuario.  
  2. *Coste de desarrollo y velocidad:* Permite un desarrollo ágil de la interfaz y la lógica de usuario sin necesidad de pasos intermedios de compilación pesados en el cliente.
- **Caso B (propuesta en ensamblador):**  
  **Rechazo la propuesta.**  
  **Justificación:**  
  1. *Falta de portabilidad:* El código en ensamblador está acoplado a una arquitectura de procesador específica (x86, ARM), imposibilitando su ejecución universal sin reescribir todo el código.  
  2. *Coste de desarrollo ineficiente:* El esfuerzo y tiempo de desarrollo requeridos son desproporcionadamente altos para una tarea sencilla de gestión de facturas, donde lenguajes de alto nivel como Java o C# ya ofrecen un rendimiento óptimo.