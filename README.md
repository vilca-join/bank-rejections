# NOMBRE DEL PROGRAMA - rechazos-bancarios
## 📋 Descripción
Este pequeño programa simula el proceso de los rechazos que realiza un banco `rechazos-bancarios`. El obejtivo es entender como funcona la lectura de archivos [.dat] y generar como resulado un archivo tipo [.txt].

## 🛠️ Especificaciones Técnicas
- **Lugar de Ejecución:** [.exe]

## 🏗️ Lógica del Programa
1. **Inicialización:** 
    - Definicion de archivos y registros secuenciales.
    ```text
            FILE-CONTROL.
            SELECT ARCHIVO-ENTRADA ASSIGN TO "../data/transacciones.dat"
                ORGANIZATION IS LINE SEQUENTIAL.
            SELECT REPORTE-RECHAZOS ASSIGN TO "../data/rechazo.txt"
                ORGANIZATION IS LINE SEQUENTIAL.
    ```
    - INPUT-OUTPUT SECTION: Aca le decimos como se comunicara nuestro programa con otros archivos del exterior.(Leer y escribir archivos)

    - Utilizamos FILE-CONTROL parrafo ENVIRONMENT DIVISION para definir los archivos que usara mi programa.
    - SELECT: Aqui es donde asignamos el nombre del archivo que usaremos dentro de la aplicacion y este estara vinculado con archivo exterior.
    - Parrafo DATA DIVISON dentro FILE SECTION. codificaremos un FD de entrada de la descripcion del archivo
    ```text
    FILE SECTION.
       FD  ARCHIVO-ENTRADA.
       01  REG-TRANSACCION.
           05  ID-CLIENTE          PIC 9(08).
           05  MONTO-TRANSAC       PIC 9(07)V99.
           05  SALDO-DISP          PIC 9(07)V99.

       FD  REPORTE-RECHAZOS.
       01  REG-REPORTE             PIC X(80).
    ```
    - Seguimos en DATA DIVISION en WORKING-STORAGE SECTION con normalidad declaramos nuestras variables de entrada y salida
    ```text
    01  WS-FLAGS.
        05  FS-FIN-ARCHIVO  PIC X(01)   VALUE   'N'.
            88  EOF         VALUE   'S'.
            88  NOT-EOF     VALUE   'N'.      
    01  WS-REGISTRO-DETALLE.
        05  WS-ID-CLIENTE   PIC 9(08).
        05  WS-MONTO        PIC 9(07)V99.
        05  WS-SALDO-ACTUAL PIC 9(07)V99.

    01  WS-CONSTANTES.
        05  ERR-SALDO-INSUF PIC X(30) 
        VALUE "RECHAZO: SALDO INSUFICIENTE ".
        05  ERR-CUENTA-BLOQ PIC X(30)
        VALUE "RECHAZO: CUENTA BLOQUEADA ".
        05  ERR-DATOS-INVALID   PIC X(30)
        VALUE "RECHAZO: DATOS INVALIDOS ".
    ```
    
2. **Proceso Principal:** 

    - Parrafo `PROCEDURE DIVISION.` llevaremos a cabo una programacion estructurada, o mas conocida por COBOL como `Diseño descendente(Top-Down Design)`.
    - Modularizamos con los siguientes nombres:
    ```text
    000-CONTROL-MAESTRO.
        >* AQUI VA EL CONTENIDO DEL MODULO.          

    100-INICIAR-PROGRAMA.
        >* AQUI VA EL CONTENIDO DEL MODULO.

    150-LEER-REGISTRO.
        >* AQUI VA EL CONTENIDO DEL MODULO.

    200-PROCESAR-ARCHIVO.
        >* AQUI VA EL CONTENIDO DEL MODULO.

    300-FINALIZAR-PROGRAMA.
        >* AQUI VA EL CONTENIDO DEL MODULO.
    ```
    - Accederemos a estos modulos mediante los `PERFORM`
    - 100-INICIAR-PROGRAMA: Aqui preparamos y hacemos la lectura inical del programa
    `INPUT ARCHIVO-ENTRADA` Le decimos al sistema que vamos a leer datos de ese archivo que tenemos guardado en nuestra estructura `"../data/transacciones.dat"`.
    `INPUT ARCHIVO-ENTRADA` Crea un archivo de salida, osea sobrescribes un archivo existente 
    - 150-LEER-REGISTRO: Usamos la sentencia `READ` para hacer la lectura del `ARCHIVO-ENTRADA`, si en caso el archivo no tiene contendio se activara una banderita EOF (End of File) de inmediato con un `SET EOF TO TRUE`. Caso contrario moveremos REG-TRANSACCION A WS-REGISTRO-DETALLE `MOVE REG-TRANSACCION TO WS-REGISTRO-DETALLE`.
    - 200-PROCESAR-ARCHIVO: Aqui validaremos el WS-MONTO con WS-SALDO-ACTUAL `IF WS-MONTO > WS-SALDO-ACTUAL`, si es verdadera, lo que hace es volcar el registro que estamos leendo en ese momento a `WS-REGISTRO-DETALLE` y mostramos un ` DISPLAY "CLIENTE: " WS-ID-CLIENTE " || " ERR-SALDO-INSUF`, y finalizamos con un `END-IF` y volvemos a llamar al modulo `200-PROCESAR-ARCHIVOS`.
    - 300-FINALIZAR-PROGRAMA: Para finalizar llamamos a este modulo, al que solo podemos llegar cuando terminamos de leer todos los registros del archivo [.dat].

3. **Finalización:** Imprimimos los resultado y llenamos el archivo de rechazos.

## 🚀 Cómo ejecutarlo
1. Asegúrate de tener instalado GnuCOBOL (`cobc`).
2. Compila con: `cobc -x programa.cbl`
3. Ejecuta con: `./programa.exe`

## 📊 Ejemplo de Salida
```text
---------------------------------------
CLIENTE: 00001234 || RECHAZO: SALDO INSUFICIENTE   
CLIENTE: 00005678 || RECHAZO: SALDO INSUFICIENTE   
CLIENTE: 00009999 || RECHAZO: SALDO INSUFICIENTE
PROCESO FINALIZADO CON EXITO.
---------------------------------------
```