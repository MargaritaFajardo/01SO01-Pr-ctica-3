# 01SO01-Pr-ctica-3
# Resumen
El script en BASH en Linux (CentOS) nos permite realizar tareas administrativas como visualizar todos los usuarios registrados muestra todos los grupos existentes en el sistema otra función más que cumple es enlistar las terminales activas o disponibles brindando asi visibilidad sobre las sesiones en curso no requiere configuraciones adicionales. Es una solución práctica para optimizar tareas de administración y gestión del sistema.

# Introducción
En el siguiente documento se analizó el uso de AWK y el manejo de scripts.
AWK es una herramienta potente y flexible para la manipulación de texto en la línea de comando, permite a los usuarios analizar, filtrar y procesar datos de manera eficiente utilizando patrones y acciones definidas por el usuario. Este lenguaje de programación está diseñado para trabajar con archivos de texto, facilitando tareas complejas como la generación de informes, la extracción de información específica y la transformación de datos.

El manejo de scripts en AWK es esencial para automatizar tareas repetitivas y optimizar flujos de trabajo. Un script en AWK puede leerse y ejecutarse línea por línea, permitiendo la aplicación de múltiples operaciones en un solo paso. Esto no solo mejora la eficiencia, sino que también reduce el margen de error en la ejecución manual de comandos.
## Objetivos
### Objetivo General
- Desarrollar script en bash dentro del entorno de Linux para la automatización de tareas de procesamiento de texto y extracción de información del sistema.
### Objetivos Específicos
- Automatizar la extracción y visualización de información de texto mediante scripts.
- Evaluar un correcto funcionamiento de scripts desarrollados en bash aplicando criterios de adaptabilidad a cambios futuros.
# Desarrollo
## Generar Script
### Script 1
Generar un script en bash que tenga al menos las siguientes opciones (a través de un menú):

`nano memu.sh`
### Script 2
Asignación de permisos:

`chmod u+x menu.sh`
### Script 3
- Listar los usuarios existentes:

```
listar_usuarios() {
echo "Usuarios existentes:"
cut -d: -fl /etc/passwd
read -p "ENTER para continuar"
}
```

- Listar los grupos existentes:

```
listar_grupos () {
echo "Grupos existentes:"
cut -d: -fl /etc/passwd
read -p "ENTER para continuar"
}
```

- Listar terminales disponibles:

```
listar_terminales() {
echo "Terminales disponibles:"
cat /etc/shells
read -p "ENTER para continuar
```

- Ingrese el nombre de un usuario y muestre a qué grupos pertenece:

```
mostrar_grupos_usuarios() {
read -p "Ingrese el nombre de un usuario:" usuario
if id "$usuario" &>/dev/null; then
echo "El usuario '$usuario' pertenece a los siguientes grupos:"
groups "$usuario"
else
echo "ERROR: '$usuario' no existe."
fi
read -p "ENTER para continuar"
}
```
- Ingrese el nombre de un grupo y muestre los usuarios de ese grupo:

```
mostrar_usuarios_grupo() {
read -p "Ingrese el nombre de un grupo: " grupo
if getent group "$group" &>/dev/null; then
echo "Usuarios del grupo '$grupo' son:"
getent gropu "$grupo |cut -d: -f4
else"
echo "ERROR: '$grupo' no existe."
fi
read -p "ENTER para continuar"
}
```

- Mostrar toda la información disponible respecto a un usuario, grupos, terminal, permisos y contraseña:

```
mostrar_info_usuario() {
read -p "Ingresa el nombre de un usuario: " usuario
f id "$usuario" &>/dev/null; then
echo "Información del usuario '$usuario':"
echo "Grupos: $(groups "$usuario")"
echo "Terminal: $(who | grep "$usuario" | awk '{print $2}' | head -n 1)"
echo "Permisos: $(ls -l /home/$usuario)"
echo "Contraseña: $(sudo chage -l $usuario | grep 'Passwd last changed')"
else
echo "ERROR: '$usuario' no existe."
fi
read -p "ENTER para continuar
}"
```

### Script 4
- Bucle que presenta el menú para el usuario y que así elija la opción que desee hasta que elija la opción salir y se cierra el bucle.

```
while true;
do
  echo "Seleccione una opción del menú:"
  echo "1. Listar los usuarios existentes en el sistema"
  echo "2. Listar todos los grupos existentes en el sistema"
  echo "3. Listar las terminales disponibles en el sistema"
  echo "4. Mostrar los grupos a los que pertenece un usuario"
  echo "5. Mostrar los usuarios de un grupo"
  echo "6. Mostrar toda la información de un usuario"
  echo "7. Salir"
  read -p "Ingrese una opción (1-7): " opcion
  case $opcion in
    1) listar_usuarios ;;
    2) listar_grupos ;;
    3) listar_terminales ;;
    4) mostrar_grupos_usuario ;;
    5) mostrar_usuarios_grupo ;;
    6) mostrar_info_usuario ;;
    7) echo "Saliendo..."; exit ;; *)
    8) echo "Opción no valida. Intente de nuevo." ;;
  esac
done
```

## Archivo Logs
### Script 1
- Evidencia del archivo logs subido al servidor.


### Script 2
Para poder ver los nombres de los usuarios sin comillas se utiliza el comando: `sed '/"//g'`
- Filtrado para ver la lista con valores únicos de ¨Nombre completo de usuario¨

`awk -F, 'NR > 1 {print $2}' logs.csv | sort | uniq | sed '/"//g'`

- Filtrado para ver la lista con valores únicos de ¨Nombre de evento¨

`awk -F, 'NR > 1 {print $6}' logs.csv | sort | uniq | sed '/"//g'`

- Filtrado para ver la lista con valores únicos de ¨Dirección IP¨
- 
`awk -F, 'NR > 1 {print $9}' logs.csv | sort | uniq | sed '/"//g'`

- Para mostrar estos tres datos se utiliza el siguiente código.

`awk -F, 'NR > 1 {print $2, $6, $9}' logs.csv | sort | uniq | sed '/"//g'`


### Script 3
- Se seleccione un nombre completo de usuario (por medio de menú o ingreso desde teclado) y de tal usuario visualice ¨Nombre de evento¨ y la marca de tiempo (hora y fecha)

```
awk -F ',' '$2 ~ /JOSSELINE DAYANNA ZHANGALLIMBAY GUAMAN/ {gsub(/"/, "", $1); gsub(/"/, "", $6); OFS="\t"; print $1, $6}' logs.csv

```

### Script 4
-  De esa selección generar un archivo de salida separado por ¨;¨ con el nombre de archivo ¨usuario-fechaActual.csv¨
Para lograr esto se creó un script llamado _newscript.sh_ en el cual se escribió el siguente código:

```
fecha=$(date +"%d_%m_%Y")

usuario="JOSSELINE DAYANA ZHANGALLIMBAY GUAMAN"

nombre_archivo=$(echo "$usuario" | sed 's/ /_/g')-$fecha.csv
awk -F',' '$2 ~ /JOSSELINE DAYANNA ZHANGALLIMBAY GUAMAN/ {gsub(/"/, "", $1); gsub(/"/, "",>
echo "Archivo guardado como $nombre_archivo"

```

- Se le da permisos con el comando: `chmod u+x newscript.sh`

- Para el envío del archivo fuera del servidor y poder abrirlo en Windows mediante MobaXterm.


# Conclusiones
- Se llegó a la conclusión de que AWK es una herramienta muy eficiente en el procesamiento y el análisis en los archivos de texto.
- Se pudo concluir que el uso de scripts en AWK permite la automatización de tareas repetitivas, ahorrando espacio y reduciendo errores.

# Recomendaciones
- Separar el script en funciones dedicadas para cada tarea, como listar usuarios, validar la existencia de un usuario, filtrar registros, etc. Esto mejora la organización y facilita el mantenimiento del código.
- Implementar restricciones para que solo usuarios autorizados puedan ejecutar el script. Verifica el usuario con el comando `whoami` para asegurarte de que solo las personas adecuadas tengan acceso.
- Documenta cada función del script con comentarios detallados. Esto ayuda a que otros usuarios o administradores puedan entender el propósito y funcionamiento de cada sección, facilitando futuras modificaciones.
- Asegúrate de que el script tenga los permisos de ejecución adecuados utilizando el comando `chmod +x script.sh`. Esto garantiza que el script se pueda ejecutar sin problemas.
- Algunas tareas dentro del script pueden requerir privilegios de superusuario. Utiliza `sudo` para ejecutar comandos que interactúan con archivos del sistema, asegurando que el script tenga los permisos necesarios.
- Verifica los permisos, propietarios y grupos de los archivos utilizando los comandos `ls -l` y `stat`. Esto te permitirá asegurarte de que los archivos tienen las configuraciones correctas y solucionar posibles problemas de permisos.

# Referencias
[1] Estructura CASE. (s/f). Desarrolloweb.com. Recuperado el 4 de enero de 2025, de https://desarrolloweb.com/articulos/estructura-case-vbscript.html
