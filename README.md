# NOMBRE DEL PROGRAMA - rechazos-bancarios
## 📋 Descripción
Este pequeño programa simula el proceso de los rechazos que realiza un banco `rechazos-bancarios`. El obejtivo es entender como funcona la lectura de archivos [.dat] y generar como resulado un archivo tipo [.txt].

## 🛠️ Especificaciones Técnicas
- **Lugar de Ejecución:** [.exe]

## 🏗️ Lógica del Programa
1. **Inicialización:** Armaremos la estructura de los datos en WORKING-STORAGE SECTION [.dat]
    - INPUT-OUTPUT SECTION: Aca le decimos como se comunicara nuestro programa con otros archivos del exterior.(Leer y escribir archivos)
    - FILE-CONTROL: Aqui es donde mapearemos y conectaremos los nombres logicos que usa el codigo con los nombres fisicos de los archivos de mi computadora.
    - SELECT: Aqui es donde asignamos 
2. **Proceso Principal:** Calcular el factorial de un numero, haciendo uso de `PERFORM VARYING`.
ejemplo: 5! = 5*4*3*2*1 = 120
3. **Finalización:** Imprimimos los resultado del factorial

## 🚀 Cómo ejecutarlo
1. Asegúrate de tener instalado GnuCOBOL (`cobc`).
2. Compila con: `cobc -x programa.cbl`
3. Ejecuta con: `./programa`

## 📊 Ejemplo de Salida
```text
---------------------------------------
INGRESE UN NUMERO ENTERO:
6
---------------------------------------
RESULTADO CALCULADO:                720
---------------------------------------
```