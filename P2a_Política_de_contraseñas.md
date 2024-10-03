# Política de contraseñas
### Sara Manar Habib Alsadi


## Objetivos

* __Entender el sistema de contraseñas del sistema operativo linux__

En el sistema linux almacena y administra a través de técnicas de seguridad para garantizar la integridad de las creenciales de los usuarios. Este proceso tiene varios pasos: hashing de contraseñas, la salazón y el almacenamiento en un archivo seguro.

El hash convierte la contraseña de texto en una cadena de carácteres. Es una función que no se puede revertir para obtener la contraseña original.

La técnica salado (salt) es un valor aleatorio que se agrega a la contraseña antes del hash- Esto ayuda a proteger contra ataques donde se precalculan hashes para una cantidad grande de contraseñas.

Las contraseñas codificadas y las sales se almacenan en el archcivo /etc/shadow, que solo lo puede leer el root. Restringiendo el acceso a este archivo, Linux garantiza que los usuarios no puedan ni modificar ni ver la información de las contraseñas.

* __Entender el PAM (Plugable authentication Module)__

La autenticación PAM ayuda a proteger las organizaciones contra las ciberamenazas. Este funciona mediante una conjunto de personas, procesos y tecnologías que ofrecen visibilidad sobre quién está utilizando cuentas con privilegios y qué hacen mientras están conectados. A través de este se podrá limitar el número de usuarios que tienen acceso a funciones administrativas.

Una solución PAM identifica los procesos del sistema, las personas y todo aquello que necesita acceso con privilegios y además, especifica las directivas que se aplican.

* __Instalar y configurar la libreria _pwquality___
  
  |Política  | Score | l. mínima | Requisitos | Ejemplos |
  | -        |-      |-           |-           |-          |
  |No válida |  <40  |     -      |    -      | contra  |            
  |Débil     |  35-50|      6     | ucredit= -1, minlen= 6 | Unodosdos  
  |Media     | 50-80 |      8     | difok= 1, ucredit= -2, minlen= 8         |     COntrase1*
  |Alta      |  >80  |    10     |    minlen= 10, ucredit= -3, difok= 2       | P@bL0eXtranjero*

### Pasos para instalar y configurar el pwquality.

1. Instalamos el pwquality.

```bash 
sudo apt-get install libpam-pwquality -y
```

2. Instalamos el pwscore:

```bash 
sudo apt-get install libpwquality-tools
```

3. Modificamos el archivo de configuración. Cambiamos los parámetros para establecer las restricciones en las contraseñas. He utilizado:
   
- difok: número de carácteres.
- ucredit: número de mayúsculas permitidas.
- minlen: longitud mínima.

```bash
sudo vim /etc/security/pwquality.conf
```

1. Aplicamos las restricciones para cada política:
>[!WARNING] Anotación:
Cuando se especifica un "-" antes de un dígito, se refiere a las mayúsculas. Con el negativo le obligas.


## Débil 

```bash
 minlen = 6
 ucredit = -1
```

## Media

```bash
 minlen = 8
 ucredit = -2
 difok = 1
```

## Alta

```bash
 minlen = 10
 ucredit = -3
 difok = 2
```

5. Verificamos que el PAM esté instalado. Tiene que estar la linea del pam unix en el fichero.

```bash
sudo vim /etc/pam.d/common-password
```

```bash
password [success=1 default=ignore] pam_unix.so obscure use_authtok try_first_pass yescrypt
```

