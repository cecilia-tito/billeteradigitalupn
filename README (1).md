# Billetera Digital

Aplicación web académica de billetera digital desarrollada con Java, Jakarta Servlets y JSP. Permite consultar el saldo, registrar recargas y gastos, y visualizar el historial de movimientos. Los datos se almacenan en `HttpSession` y se reinician al finalizar la sesión.

## 1. Versiones utilizadas

- JDK: 17 (también compatible con JDK 21).
- Apache Tomcat: 10.1.x.
- Jakarta Servlet API: 6.0.0 (proporcionada por Tomcat 10.1).
- Jakarta Server Pages: 3.1 (proporcionada por Tomcat 10.1).
- Jakarta JSTL API: 3.0.0.
- Jakarta JSTL implementación: 3.0.1.
- Maven: 3.8.6 o superior recomendado.
- Bootstrap: 5.3.3 (CDN; requiere conexión a Internet para cargar los estilos).

## 2. Requisitos previos

1. Instalar JDK 17 o 21 y configurar `JAVA_HOME`.
2. Instalar Apache Maven 3.8.6 o superior y configurar `PATH`.
3. Instalar Apache Tomcat 10.1.x.
4. Verificar las instalaciones:

```bash
java -version
mvn -version
```

## 3. Compilar y generar el WAR

Desde la carpeta raíz del proyecto, donde se encuentra `pom.xml`, ejecutar:

```bash
mvn clean package
```

Maven descargará las dependencias y generará:

```text
target/BilleteraDigital.war
```

## 4. Desplegar en Tomcat

### Opción A: copiar el WAR

1. Detener Tomcat si está ejecutándose.
2. Copiar `target/BilleteraDigital.war` a la carpeta `webapps` de Tomcat.
3. Iniciar Tomcat.
4. Abrir en el navegador:

```text
http://localhost:8080/BilleteraDigital/
```

Tomcat desplegará automáticamente la aplicación.

### Opción B: desde la consola de Tomcat

También se puede desplegar el WAR desde la aplicación Manager de Tomcat, si está habilitada y configurada.

## 5. Ejecución desde un IDE

Importar el proyecto como proyecto Maven existente, configurar un servidor Apache Tomcat 10.1.x y ejecutar el artefacto WAR. La URL esperada es `http://localhost:8080/BilleteraDigital/`.

## 6. Funcionalidades y reglas

- Saldo inicial: S/ 250.00.
- Recarga: suma el monto al saldo.
- Gasto: resta el monto si existe saldo suficiente.
- Se rechazan montos vacíos, no numéricos, cero, negativos y con más de dos decimales.
- Se rechazan tipos de movimiento distintos de `Recarga` y `Gasto`.
- Las operaciones rechazadas no actualizan saldo ni historial.
- Los movimientos se muestran del más reciente al más antiguo.
- El saldo y el historial se guardan en la sesión HTTP; no existe persistencia en base de datos.

## 7. Pruebas manuales sugeridas

Partiendo del saldo inicial de S/ 250.00:

| Acción | Resultado esperado |
|---|---:|
| Recarga S/ 100.00 | S/ 350.00 |
| Gasto S/ 50.00 | S/ 300.00 |
| Gasto S/ 400.00 | Rechazado: saldo insuficiente; saldo permanece S/ 300.00 |
| Recarga S/ 0.00 | Rechazado |
| Gasto S/ -20.00 | Rechazado |
| Monto con letras | Rechazado |
| Monto con tres decimales | Rechazado |

## 8. Estructura del proyecto

```text
BilleteraDigital/
├── pom.xml
├── README.md
└── src/
    └── main/
        ├── java/com/billetera/
        │   ├── model/Movimiento.java
        │   └── servlet/BilleteraServlet.java
        └── webapp/
            ├── index.jsp
            └── WEB-INF/vistas/billetera.jsp
```

## 9. Publicar en GitHub

Crear un repositorio vacío en GitHub y, desde esta carpeta, ejecutar:

```bash
git init
git add .
git commit -m "Entrega inicial de Billetera Digital"
git branch -M main
git remote add origin https://github.com/USUARIO/BilleteraDigital.git
git push -u origin main
```

Reemplazar `USUARIO` por el nombre de usuario real de GitHub y utilizar la URL del repositorio creado.

## 10. Notas

- No se utiliza Spring ni otro framework de aplicación.
- Tomcat 10.1 utiliza paquetes `jakarta.*`; no cambiar los imports a `javax.*`.
- Bootstrap se carga desde CDN.
- Esta implementación es una base académica: no incluye autenticación, base de datos ni persistencia después de cerrar la sesión.
