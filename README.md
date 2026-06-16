# Simulador de Gestión de Solicitudes de Vacaciones

## 1. Descripción general

Esta plataforma es un simulador desarrollado en Python que representa el funcionamiento de un chatbot para gestionar solicitudes de vacaciones dentro de una empresa ficticia llamada **LogiNEA S.A.**

El sistema se ejecuta por consola, es decir, desde una ventana de comandos o terminal. No utiliza página web, Telegram ni WhatsApp. Su objetivo es simular cómo un empleado puede consultar sus días disponibles y generar una solicitud de vacaciones, mientras el sistema valida automáticamente si corresponde aprobarla o rechazarla.

La información se guarda en archivos CSV, que funcionan como una base de datos simulada.

---

## 2. Archivos necesarios

Para que el simulador funcione correctamente, la carpeta del proyecto debe contener los siguientes elementos:

```text
proyecto_vacaciones/
│
├── app_licencias_vacaciones.py
│
└── datos/
    ├── empleados.csv
    └── solicitudes.csv
```

### Archivo principal

```text
app_licencias_vacaciones.py
```

Es el programa principal. Desde este archivo se inicia el simulador.

### Carpeta `datos`

Dentro de esta carpeta se guardan los archivos CSV utilizados por el sistema.

### Archivo `empleados.csv`

Contiene la información de los empleados.

Campos esperados:

```text
legajo,nombre,apellido,sector,dias_disponibles,estado
```

Ejemplo:

```text
1001,Juan,Perez,Sistemas,14,ACTIVO
```

### Archivo `solicitudes.csv`

Contiene el historial de solicitudes de vacaciones.

Campos esperados:

```text
id_solicitud,legajo,sector,fecha_inicio,fecha_fin,dias_solicitados,estado,motivo_rechazo
```

Ejemplo de solicitud aprobada:

```text
1,1001,Sistemas,2026-07-15,2026-07-20,6,APROBADA,
```

Ejemplo de solicitud rechazada:

```text
2,1002,Sistemas,2026-07-15,2026-07-20,6,RECHAZADA,Conflicto de cobertura del sector.
```

---

## 3. Requisitos para usar el sistema

Para ejecutar el simulador se necesita:

1. Tener Python instalado en la computadora.
2. Tener la carpeta del proyecto completa.
3. Verificar que exista la carpeta `datos`.
4. Verificar que dentro de `datos` estén los archivos `empleados.csv` y `solicitudes.csv`.

No es necesario instalar librerías externas. El programa utiliza herramientas incluidas en Python.

---
## 4. Uso de GitHub y PAT

El proyecto fue versionado utilizando Git y GitHub. Para la autenticación se utilizó un Personal Access Token de tipo fine-grained, configurado únicamente para este repositorio y con permisos mínimos necesarios para lectura y escritura del contenido.

El token fue utilizado solo como credencial de autenticación al realizar operaciones de push mediante HTTPS. No fue incluido en el código fuente, archivos CSV, documentación ni historial de commits.
## 5. Cómo ejecutar el programa

### Paso 1: Abrir la carpeta del proyecto

Ubicar la carpeta donde se encuentra el archivo principal del sistema.

Ejemplo:

```text
proyecto_vacaciones/
```

### Paso 2: Abrir una terminal o consola

Puede usarse:

- Terminal de Visual Studio Code.
- Símbolo del sistema de Windows.
- PowerShell.
- Terminal de Linux o Mac.

### Paso 3: Ejecutar el programa

Dentro de la carpeta del proyecto, escribir el siguiente comando:

```bash
python app_licencias_vacaciones.py
```

Si el comando anterior no funciona, probar con:

```bash
python3 app_licencias_vacaciones.py
```

---

## 6. Funcionamiento general del simulador

Al iniciar el programa, se muestra un mensaje de bienvenida y luego el **menú inicial**.

```text
MENÚ INICIAL
1. Ingresar legajo
2. Salir
```

El usuario debe escribir el número de la opción deseada y presionar Enter.

---

## 7. Menú inicial

### Opción 1: Ingresar legajo

El sistema solicita el número de legajo del empleado.

```text
Ingrese su legajo:
```

Luego valida si el legajo existe en el archivo `empleados.csv`.

#### Si el legajo no existe

El sistema informa el error y vuelve al menú inicial.

```text
Legajo inválido o inexistente.
```

#### Si el empleado está inactivo

El sistema informa que el empleado no puede operar y vuelve al menú inicial.

```text
El legajo pertenece a un empleado inactivo.
```

#### Si el legajo es válido

El sistema permite ingresar al menú principal.

```text
Legajo validado correctamente. Bienvenido/a.
```

### Opción 2: Salir

Finaliza el programa.

---

## 8. Menú principal

Cuando el legajo es válido y el empleado está activo, se muestra el menú principal.

```text
MENÚ PRINCIPAL
1. Consultar saldo de vacaciones
2. Generar solicitud de vacaciones
3. Salir
```

---

## 9. Consultar saldo de vacaciones

Si el usuario elige la opción 1, el sistema muestra:

- Nombre del empleado.
- Sector al que pertenece.
- Días disponibles de vacaciones.

Ejemplo:

```text
CONSULTA DE SALDO

Empleado: Juan Perez
Sector: Sistemas
Días disponibles: 14
```

Luego el sistema solicita presionar Enter para volver al menú principal.

---

## 10. Generar solicitud de vacaciones

Si el usuario elige la opción 2, el sistema inicia el proceso de carga de una solicitud.

Primero solicita la fecha de inicio:

```text
Fecha de inicio:
```

Después solicita la fecha de finalización:

```text
Fecha de fin:
```

Las fechas deben ingresarse con el formato:

```text
AAAA-MM-DD
```

Ejemplo válido:

```text
2026-07-15
```

---

## 11. Validaciones de fechas

El sistema controla que las fechas ingresadas sean correctas.

### Formato incorrecto

Si el usuario escribe una fecha incompleta o con formato incorrecto, el sistema no registra ninguna solicitud y pide ingresar la fecha nuevamente.

Ejemplo incorrecto:

```text
2026-10
```

Respuesta del sistema:

```text
Fecha ingresada incorrecta. Debe usar el formato AAAA-MM-DD. Intente nuevamente.
```

### Fecha de inicio posterior a la fecha final

Si la fecha de inicio es posterior a la fecha de fin, el sistema solicita corregir el dato.

Ejemplo incorrecto:

```text
Fecha inicio: 2026-08-10
Fecha fin: 2026-08-05
```

### Fecha pasada

Si la fecha de inicio ya pasó, el sistema no permite continuar con la solicitud.

---

## 12. Reglas de aprobación y rechazo

Una vez que las fechas son válidas, el sistema evalúa las reglas de negocio.

### Solicitud aprobada

La solicitud se aprueba cuando:

1. El empleado está activo.
2. Tiene días disponibles.
3. El saldo alcanza para la cantidad de días solicitados.
4. No existe otro empleado del mismo sector con vacaciones aprobadas en fechas superpuestas.

Cuando se aprueba una solicitud:

- Se registra en `solicitudes.csv`.
- Se descuenta el saldo del empleado en `empleados.csv`.
- Se muestra un mensaje de aprobación.

Ejemplo:

```text
SOLICITUD APROBADA
Días solicitados: 5
Nuevo saldo disponible: 9
```

### Solicitud rechazada

La solicitud se rechaza y se guarda en `solicitudes.csv` cuando ocurre alguno de estos casos:

1. Falta de días disponibles.
2. Saldo insuficiente.
3. Conflicto de cobertura del sector.

---

## 13. Motivos de rechazo

### Falta de días disponibles

Ocurre cuando el empleado ya no tiene días disponibles para solicitar vacaciones.

Ejemplo:

```text
Días disponibles: 0
```

En este caso, la solicitud queda registrada como:

```text
RECHAZADA
Motivo: Falta de días disponibles.
```

### Saldo insuficiente

Ocurre cuando el empleado tiene algunos días disponibles, pero solicita más días de los que posee.

Ejemplo:

```text
Días disponibles: 7
Días solicitados: 10
```

En este caso, la solicitud queda registrada como:

```text
RECHAZADA
Motivo: Saldo insuficiente.
```

### Conflicto de cobertura del sector

Ocurre cuando ya existe otro empleado del mismo sector con una solicitud aprobada en fechas que se superponen.

Ejemplo:

- Empleado 1001 del sector Sistemas tiene vacaciones aprobadas del 2026-07-10 al 2026-07-15.
- Empleado 1002 del sector Sistemas solicita vacaciones del 2026-07-12 al 2026-07-16.

Como las fechas se superponen, el sistema rechaza la solicitud para evitar que haya más de un empleado del mismo sector ausente en el mismo período.

En este caso, la solicitud queda registrada como:

```text
RECHAZADA
Motivo: Conflicto de cobertura del sector.
```

---

## 14. Diferencia entre error de carga y rechazo

Es importante diferenciar estos dos casos.

### Error de carga

Un error de carga ocurre cuando el usuario ingresa mal un dato.

Ejemplos:

- Fecha con formato incorrecto.
- Fecha incompleta.
- Fecha de inicio posterior a la fecha final.
- Fecha pasada.

Estos casos no se guardan como solicitudes rechazadas, porque el sistema todavía no tiene una solicitud válida para evaluar. Simplemente le pide al usuario corregir el dato.

### Rechazo de solicitud

Un rechazo ocurre cuando los datos ingresados son válidos, pero no cumplen una regla de negocio.

Ejemplos:

- El empleado no tiene días disponibles.
- El empleado solicita más días de los que tiene.
- Ya existe otro empleado del mismo sector con vacaciones aprobadas en esas fechas.

Estos casos sí se guardan en `solicitudes.csv` con estado `RECHAZADA` y su motivo correspondiente.

---

## 15. Archivos que se modifican durante el uso

### Cuando se consulta saldo

No se modifica ningún archivo.

### Cuando se aprueba una solicitud

Se modifican dos archivos:

1. `solicitudes.csv`: se agrega la solicitud aprobada.
2. `empleados.csv`: se descuentan los días utilizados.

### Cuando se rechaza una solicitud

Se modifica un archivo:

1. `solicitudes.csv`: se agrega la solicitud rechazada con su motivo.

En este caso, `empleados.csv` no se modifica porque no se descuentan días.

---

## 16. Recomendaciones antes de probar el sistema

Antes de ejecutar pruebas, se recomienda:

1. Hacer una copia de seguridad de los archivos CSV.
2. Revisar que los encabezados de los CSV estén escritos correctamente.
3. No borrar la carpeta `datos`.
4. No cambiar los nombres de los archivos.
5. No modificar manualmente los saldos mientras el sistema está abierto.

---

## 17. Problemas comunes

### El programa dice que no encuentra empleados.csv

Verificar que exista esta ruta:

```text
datos/empleados.csv
```

### El programa dice que no encuentra solicitudes.csv

Verificar que exista esta ruta:

```text
datos/solicitudes.csv
```

### El legajo aparece como inválido

Revisar que el legajo exista en `empleados.csv` y que esté escrito igual que en el archivo.

### El empleado no puede ingresar

Revisar que el campo `estado` del empleado figure como:

```text
ACTIVO
```

### La solicitud se rechaza por conflicto de cobertura

Revisar en `solicitudes.csv` si ya existe otra solicitud aprobada del mismo sector en fechas superpuestas.

---

## 18. Cierre del sistema

Para finalizar el sistema, se debe elegir la opción **Salir** desde el menú inicial o desde el menú principal.

El sistema mostrará un mensaje de despedida y terminará la ejecución.

---

## 19. Resumen del flujo de uso

```text
Inicio del programa
↓
Menú inicial
↓
Ingresar legajo
↓
Validación de empleado
↓
Menú principal
↓
Consultar saldo o generar solicitud
↓
Validación de fechas y reglas de negocio
↓
Solicitud aprobada o rechazada
↓
Registro en archivos CSV
↓
Fin o nueva operación
```

---

## 20. Aclaración final

Este sistema es una simulación académica. Su objetivo es representar cómo podría automatizarse un proceso administrativo de Recursos Humanos mediante un chatbot básico, utilizando Python y archivos CSV como base de datos simulada.
