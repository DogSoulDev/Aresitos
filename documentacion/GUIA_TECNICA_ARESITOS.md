
# 🛡️ Guía Técnica Completa de ARESITOS

## Vista general del flujo de ARESITOS


Este recorrido visual muestra el flujo completo de uso de ARESITOS, desde la instalación en terminal hasta la generación de reportes, ilustrando cada pantalla y funcionalidad clave:






![Selector de herramientas Kali](../aresitos/recursos/capturas/3_herramientas.png)


**Selector de herramientas Kali:**
Tras el inicio de sesión, el usuario accede a un panel visual donde puede seleccionar y lanzar las principales herramientas de Kali Linux integradas en ARESITOS, facilitando la gestión centralizada de utilidades de ciberseguridad.



ARESITOS es una suite profesional de ciberseguridad para Kali Linux, con módulos de escaneo, SIEM, FIM, cuarentena y dashboard integrados. Su arquitectura es 100% Python nativo, integrando herramientas de Kali y priorizando la seguridad, modularidad y extensibilidad. Permite la gestión avanzada de privilegios y la integración de utilidades nativas.





## Estructura técnica y módulos principales

![Dashboard principal](../aresitos/recursos/capturas/4_dashboard.png)

**Dashboard principal:**
El panel principal centraliza la navegación y el estado general del sistema, mostrando accesos rápidos a los módulos de escaneo, SIEM, FIM, cuarentena, reportes y configuración, así como información de estado y alertas.

### Estructura técnica del proyecto


- `aresitos/modelo/`: Modelos de datos y lógica de negocio. Cada archivo implementa la gestión de datos, acceso a bases SQLite, validaciones, operaciones CRUD y lógica específica de cada módulo (escaneo, FIM, SIEM, cuarentena, reportes, dashboard, diccionarios, wordlists). Ejemplo: `modelo_fim.py` gestiona la integridad de archivos críticos, calcula hashes, almacena y compara resultados; `modelo_reportes.py` gestiona la generación, almacenamiento y exportación de reportes.
- `aresitos/vista/`: Interfaz gráfica Tkinter. Cada archivo define una pantalla principal como clase `tk.Frame`, con widgets, paneles, terminal integrada, navegación y controles visuales. Ejemplo: `vista_dashboard.py` muestra métricas y logs en tiempo real; `vista_reportes.py` permite generar, visualizar y exportar informes profesionales; `vista_monitoreo.py` gestiona procesos, red y archivos sospechosos.
- `aresitos/controlador/`: Orquestación y lógica de control. Cada controlador coordina la interacción entre la vista y el modelo, ejecuta herramientas de Kali, comandos de sistema y gestiona tareas en segundo plano. Ejemplo: `controlador_escaneo.py` ejecuta escaneos de red y vulnerabilidades; `controlador_fim.py` orquesta el monitoreo de integridad y la cuarentena; `controlador_reportes.py` gestiona la generación y exportación de reportes.
- `aresitos/utils/`: Utilidades transversales para seguridad, permisos, threading, logging y helpers. Ejemplo: `sudo_manager.py` gestiona privilegios sudo/root y la ejecución segura de comandos; `sanitizador_archivos.py` valida y sanitiza rutas y nombres; `logger_aresitos.py` centraliza el logging; `thread_safe_gui.py` implementa control seguro de hilos en la UI.
- `aresitos/recursos/`: Imágenes, iconos, capturas de pantalla y recursos visuales utilizados en la interfaz y documentación.
- `data/`: Bases de datos SQLite (FIM, SIEM, cuarentena), wordlists, diccionarios, cheatsheets y archivos de cuarentena. Ejemplo: `fim_kali2025.db` almacena hashes y eventos de integridad; `cuarentena_kali2025.db` gestiona archivos sospechosos; subcarpetas para wordlists y diccionarios personalizables.
- `configuración/`: Archivos de configuración global en JSON, textos en castellano, mapas de navegación y traducciones. Ejemplo: `aresitos_config_completo.json` contiene la configuración principal; `textos_castellano_corregido.json` centraliza los textos y mensajes de la interfaz.
- `documentacion/`: Documentos técnicos, guías de instalación, arquitectura, auditoría y manuales de uso. Ejemplo: `GUIA_TECNICA_ARESITOS.md` (esta guía), `GUIA_INSTALACION.md` (instalación paso a paso).
- `logs/`: Archivos de logs de errores, actividad y resultados de escaneo. Ejemplo: `aresitos_errores.log` registra eventos críticos y auditoría.
- `reportes/`: Reportes generados por el usuario en formatos TXT, JSON y PDF, siguiendo la plantilla profesional ISO/IEC 27001.

---

---
### 2.6. Módulo de gestión de wordlists y diccionarios
<div align="center">
	<img src="../aresitos/recursos/capturas/10_wordlistsydiccionarios.png" alt="Gestión de wordlists y diccionarios" width="500" />
</div>
**Funcionalidad:** Facilita la creación, validación y uso de diccionarios personalizados para pruebas de fuerza bruta, auditorías de contraseñas y escaneos avanzados.
**Clases principales:** `vista_datos.py`, `modelo_wordlists.py`, `modelo_wordlists_gestor.py`, `modelo_diccionarios.py`
**Flujo técnico:**
- El modelo valida, genera y almacena los recursos.
- La vista permite cargar, actualizar y exportar listas personalizadas.

- El controlador verifica el estado de cada herramienta y gestiona la instalación.
- La vista muestra el progreso y el resultado de cada acción.

---



## Mapeo de conexiones MVC

El proyecto sigue el patrón Modelo-Vista-Controlador (MVC), donde cada vista está vinculada a uno o varios controladores y modelos según la funcionalidad. La vista orquesta la interacción con el usuario y delega la lógica al controlador, que a su vez gestiona los datos a través del modelo correspondiente.


| Vista                      | Controlador                | Modelo relacionado                |
|----------------------------|----------------------------|------------------------------------|
| vista_principal.py         | controlador_principal.py   | modelo_principal.py                |
| vista_dashboard.py         | controlador_dashboard.py   | modelo_dashboard.py                |
| vista_escaneo.py           | controlador_escaneo.py     | modelo_escaneador.py, modelo_escaneador_base.py |
| vista_auditoria.py         | controlador_auditoria.py   | modelo_diccionarios.py, modelo_principal.py     |
| vista_fim.py               | controlador_fim.py         | modelo_fim.py, modelo_fim_base.py  |
| vista_siem.py              | controlador_siem.py        | modelo_siem.py, modelo_siem_base.py|
| vista_monitoreo.py         | controlador_monitoreo.py   | modelo_monitor.py                  |
| vista_reportes.py          | controlador_reportes.py    | modelo_reportes.py                 |
| vista_herramientas_kali.py | controlador_herramientas.py| modelo_principal.py                |
| vista_login.py             | controlador_principal.py   | modelo_principal.py                |
| vista_datos.py             | controlador_principal.py   | modelo_principal.py                |




## Inicialización y ciclo interno


El proceso de arranque de ARESITOS está centralizado en `main.py`, que verifica el entorno, configura permisos, valida dependencias y lanza la interfaz gráfica. El ciclo MVC se inicia creando instancias de modelo, vista y controlador, conectando cada componente para garantizar la separación de responsabilidades y la robustez del sistema.

**Flujo de inicialización:**
1. Verificación de entorno (Kali Linux, dependencias, permisos, tkinter)
2. Configuración de permisos básicos en carpetas de datos y configuración
3. Lanzamiento de la pantalla de login (`vista_login.py`)
4. Tras autenticación, inicialización de la vista principal (`vista_principal.py`), modelo principal (`modelo_principal.py`) y controlador principal (`controlador_principal.py`)
5. Carga dinámica de módulos y paneles según la navegación del usuario
6. Ejecución de tareas en segundo plano mediante hilos seguros (`threading.Thread`)
7. Logging centralizado y auditoría de eventos

**Ejemplo técnico de inicialización MVC:**
```python
from aresitos.modelo.modelo_principal import ModeloPrincipal
# ...código de inicialización...
```

## Logging centralizado

## Threading seguro y concurrencia
	pass
hilo = threading.Thread(target=tarea_larga)

## Terminal integrada
```
Características avanzadas:
- Redirección de stdout/stderr y logging thread-safe
- Controles para limpiar, activar/desactivar logs, abrir terminal externo

## Exportación de reportes profesionales ISO/IEC 27001
El módulo de reportes de ARESITOS permite generar, visualizar y exportar informes técnicos y ejecutivos siguiendo la estructura oficial ISO/IEC 27001. El usuario puede seleccionar los módulos y opciones a incluir, y el sistema recopila los datos, valida la estructura y exporta el informe en el formato deseado:
- **TXT:** Informe plano profesional, estructurado y listo para auditoría.
- **JSON:** Exportación estructurada para análisis automatizado y trazabilidad.
- **PDF:** Exportación avanzada usando herramientas nativas de Kali (enscript, ps2pdf).

### Flujo técnico de generación y exportación
1. El usuario configura los módulos y opciones en la vista de reportes (`vista_reportes.py`).
2. La vista permite visualizar, comparar y exportar los informes.
3. Si se activa la opción de terminales externas, se incluye una sección detallada con la información de todas las terminales abiertas en Kali Linux.

### Ejemplo técnico de generación de reporte TXT
```python
line = lambda c: c*80
secciones = [
	line('='),
	"INFORME DE INCIDENTE DE SEGURIDAD DE LA INFORMACIÓN - ISO/IEC 27001", line('='),
	f"Organización: {campos.get('organizacion','')}",
	# ...secciones del informe...
	line('='),
	"Reporte generado por ARESITOS conforme a ISO/IEC 27001"
]
return '\n'.join(secciones)
```

### Ejemplo técnico de exportación PDF
```python
from aresitos.utils.sudo_manager import get_sudo_manager
sudo_manager = get_sudo_manager()
res1 = sudo_manager.execute_sudo_command(f"enscript -B -o '{tmp_ps_path}' '{tmp_txt_path}'")
res2 = sudo_manager.execute_sudo_command(f"ps2pdf '{tmp_ps_path}' '{pdf_destino}'")
```

Ventajas técnicas:
- Cumplimiento normativo y trazabilidad profesional
- Exportación multiplataforma y formatos estándar
- Integración de contexto real (terminales externas, logs, módulos activos)

---




## Proceso de instalación y requisitos

### Instalación rápida
```bash
git clone https://github.com/DogSoulDev/aresitos.git
cd aresitos
chmod +x configurar_kali.sh
sudo ./configurar_kali.sh
python3 main.py
```

### Instalación manual (opcional)
```bash
sudo apt update
sudo apt install python3 python3-tk python3-venv nmap masscan nuclei gobuster ffuf feroxbuster wireshark autopsy sleuthkit git curl wget sqlite3
python3 main.py
```

### Requisitos mínimos recomendados (Kali Linux)

### Requisitos mínimos reales (Kali Linux, medidos en pruebas reales)
- **CPU:** 1 núcleo x86_64 (recomendado 2 núcleos para multitarea)
- **RAM:** 1 GB libre (uso típico bajo, recomendado 2 GB para análisis forense o escaneos intensivos)
- **Almacenamiento:** ~25 MB libres para instalación base, más 20 MB adicionales para datos, reportes y logs
- **Python:** 3.8 o superior
- **Paquetes Python:** Solo biblioteca estándar (`tkinter`, `sqlite3`, `hashlib`, `subprocess`, `threading`, `os`, `json`, `logging`)
- **Herramientas externas:** nmap, masscan, nuclei, gobuster, ffuf, feroxbuster, wireshark, autopsy, sleuthkit, hashdeep, testdisk, bulk-extractor, dc3dd, guymager, git, curl, wget, sqlite3, inotify-tools, chkrootkit, rkhunter, clamav, yara, linpeas (todas instalables vía APT en Kali Linux)

---

- Comandos de sistema ejecutados en hilos separados.
- Resultados mostrados en la UI y almacenados en modelos.
- Archivos sospechosos validados y movidos a cuarentena.
- Hashes calculados y registrados.
- Correlación de eventos, análisis de logs, monitoreo de integridad, alertas en tiempo real.
- Terminal integrada permite ejecutar comandos validados desde la UI, mostrando resultados y logs en tiempo real.

---





## Buenas prácticas, referencias técnicas y recursos recomendados

ARESITOS sigue las mejores prácticas de seguridad y desarrollo profesional, aplicando recomendaciones de organismos y guías reconocidas internacionalmente. Se recomienda consultar y aplicar los siguientes principios y recursos:

### Recomendaciones de seguridad y desarrollo
- Validación y sanitización exhaustiva de entradas, rutas y archivos
- Gestión de privilegios centralizada y controlada (`sudo_manager.py`, `gestor_permisos.py`)
- Threading seguro y control de concurrencia (`thread_safe_gui.py`)
- Prohibición de prácticas inseguras (`os.system`, `eval`, `exec`, `shell=True`)
- Logging y auditoría centralizados (`logger_aresitos.py`)
- Documentación técnica y auditoría actualizada en la carpeta `documentacion/`
- Exportación de evidencias y reportes solo tras validación y confirmación

### Referencias técnicas y recursos oficiales
- [OWASP Top Ten](https://owasp.org/www-project-top-ten/): Referencia para evitar vulnerabilidades comunes
- [Guía de Hardening de Kali Linux](https://www.kali.org/docs/general-use/securing-kali-linux/): Recomendaciones oficiales para asegurar el entorno
- [Guía de Seguridad de Python](https://docs.python.org/3/howto/security.html): Prácticas recomendadas para desarrollo seguro en Python


Todos los comandos del sistema se ejecutan usando `subprocess.run` con validación previa y sin `shell=True` para evitar riesgos de inyección:

```python
	print(f"Error: {resultado.stderr}")
```
```python
import hashlib
		return hashlib.sha256(f.read()).hexdigest()
hash_archivo = calcular_hash_sha256('/etc/passwd')
print(f"SHA256: {hash_archivo}")
```

- [hashlib — Documentación oficial de Python](https://docs.python.org/3/library/hashlib.html)


### 12.3. Hilos seguros para tareas en segundo plano

El monitoreo y escaneo se realiza en hilos separados usando `threading` para no bloquear la interfaz:

```python
import threading

def tarea_larga():
	# ...código de escaneo o monitoreo...
	pass

hilo = threading.Thread(target=tarea_larga)
hilo.daemon = True
hilo.start()
```

- [threading — Documentación oficial de Python](https://docs.python.org/3/library/threading.html)



### 12.4. Validación y saneamiento de comandos

**Importante:** En la implementación real de Aresitos, **no existe validación ni filtrado de comandos en la terminal integrada**. El usuario puede ejecutar cualquier comando, y la responsabilidad de la seguridad recae en el propio usuario y en la protección de procesos críticos implementada en la detención de procesos.

**Fragmento real de ejecución de comandos (vista_monitoreo.py):**
```python
def ejecutar_comando_terminal(self, comando):
    resultado = subprocess.run(comando, capture_output=True, text=True, shell=True)
    # ...

**SudoManager:**
from aresitos.utils.sudo_manager import SudoManager
sudo = SudoManager()
resultado = sudo.ejecutar_comando_privilegiado(comando)
```


**No existe lista negra ni validación de comandos en la terminal.**



### 12.5. Patrón MVC en la arquitectura de Aresitos


```python

### 12.6. Logging centralizado y seguro

---

## Flujo técnico de escaneo y cuarentena

Tras realizar un escaneo de red, el sistema muestra todos los resultados técnicos relevantes: IPs, DNS y vulnerabilidades detectadas. La gestión de cuarentena se realiza exclusivamente desde el botón "Agregar IP a cuarentena", que permite aislar cualquier elemento detectado (IP, DNS, vulnerabilidad) o introducido manualmente por el usuario.

Este nuevo flujo elimina el antiguo botón "Mandar a cuarentena", centralizando toda la funcionalidad en una única acción y mejorando la experiencia de uso. El proceso es más intuitivo y evita errores, asegurando que ningún elemento relevante quede fuera del aislamiento.

**Ventajas del nuevo flujo:**
- Centralización de la cuarentena en un solo botón.
- Posibilidad de aislar manualmente cualquier IP, DNS o vulnerabilidad.
- Visualización técnica clara y detallada de los resultados del escaneo.
- Mayor robustez y transparencia en la gestión de amenazas.

## Nota importante sobre el terminal y la ejecución en Kali Linux

Cuando ejecutas ARESITOS desde el terminal (por ejemplo, con `python3 main.py`), el terminal permanecerá abierto mientras la aplicación esté en uso. Esto es una limitación del sistema operativo: ningún programa puede cerrar el terminal que lo inició automáticamente.

Si quieres trabajar únicamente con la interfaz gráfica y sin terminal abierto, tienes dos opciones:
- Cierra el terminal manualmente después de que se abra la ventana de ARESITOS. La aplicación seguirá funcionando sin problemas.
- Utiliza un acceso directo gráfico (.desktop) o inicia ARESITOS desde el menú de aplicaciones de Kali Linux. Así, solo tendrás abierta la ventana del programa.

Esta es una limitación estándar en Linux y no depende de ARESITOS. Se recomienda el uso del acceso directo gráfico para una experiencia óptima.
