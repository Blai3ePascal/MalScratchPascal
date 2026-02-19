## Manual para la clase

> ⚠️ Aviso educativo  
> Este proyecto es un simulador: los bloques NO ejecutan malware real.  
> Aquí explicamos qué representa cada bloque en la vida real para que el alumnado entienda conceptos  
> (y también por qué defenderse y cómo se detecta). NO es una guía para atacar.

---

### CONTROL

#### `} CERRAR_ESTRUCTURA` (`end_block`)

**En el simulador**

-   Cierra el último “ámbito” abierto (como cerrar un paréntesis grande).
    
-   Visualmente baja la sangría (indentación) y le dice al motor “ya no estamos dentro de ese IF/FOREACH/TRY”.
    

**En la vida real (qué representa)**

-   No es una técnica de malware: representa **estructura de programa**.
    
-   En un programa real, cerrar un bloque significa “a partir de aquí ya no aplican esas reglas”.
    

**Por qué importa (impacto real de entender esto)**

-   Muchos comportamientos “peligrosos” dependen de estar dentro de condiciones:
    
    -   “si tengo permisos…”
        
    -   “si hay internet…”
        
    -   “para cada archivo…”
        
-   Si no se cierran estructuras correctamente, el software real puede fallar, comportarse raro o revelar errores.
    

---

### LOGIC (estructuras de decisión, repetición y robustez)

#### `FOR_EACH_FILE` (`loop_files`)

**En el simulador**

-   Abre un ámbito que significa: “haz lo siguiente para muchos archivos”.
    
-   Potencia muchísimo acciones que necesitan repetirse (por ejemplo “cifrar muchos archivos” o “subir muchos archivos”).
    

**En la vida real**

-   Representa **recorrer el sistema de archivos** (carpetas y ficheros) para:
    
    -   buscar documentos concretos,
        
    -   robar información,
        
    -   cifrar archivos (ransomware),
        
    -   borrar evidencias,
        
    -   o elegir “objetivos valiosos” (por ejemplo, carpetas de trabajo).
        

**Cómo afecta a un ordenador (impacto)**

-   Puede causar:
    
    -   uso alto de disco (el PC va lento),
        
    -   muchos archivos cambiando de golpe,
        
    -   ventilador/CPU alto si además procesa o cifra.
        
-   Si un malware cifra archivos masivamente, el usuario puede ver:
    
    -   archivos que dejan de abrir,
        
    -   extensiones raras,
        
    -   mensajes de rescate (en ransomware real).
        

**Señales típicas / defensas**

-   Señales: una aplicación que “no toca tantos archivos” empieza a leer/escribir miles.
    
-   Defensa: control de aplicaciones, EDR/antivirus con comportamiento, copias de seguridad, mínimos privilegios.
    

---

#### `FOR_EACH_IP` (`loop_network`)

**En el simulador**

-   Abre un ámbito: “para cada IP (equipo) de una red, intenta lo siguiente”.
    
-   Hace que “propagación” tenga sentido (sin lista de objetivos, no hay a quién propagarse).
    

**En la vida real**

-   Representa **enumerar equipos de la red**: intentar ver “quién está conectado” o “qué equipos existen”.
    
-   Se usa como paso previo a:
    
    -   moverse lateralmente (pasar de un ordenador a otro),
        
    -   buscar servidores,
        
    -   o localizar “el ordenador con datos valiosos”.
        

**Cómo afecta a un ordenador (impacto)**

-   Puede generar:
    
    -   picos de tráfico de red,
        
    -   alertas del router o del firewall,
        
    -   sensación de red lenta.
        
-   A nivel organización, puede disparar alertas porque parece “un barrido” (scan) interno.
    

**Señales típicas / defensas**

-   Señales: un equipo “normal” hace conexiones a muchos equipos en pocos segundos/minutos.
    
-   Defensa: segmentación de red, firewalls internos, monitorización de tráfico, MFA y credenciales fuertes.
    

---

#### `FOR_EACH_PID` (`loop_processes`)

**En el simulador**

-   Abre un ámbito: “para cada proceso en ejecución, haz lo siguiente”.
    

**En la vida real**

-   Representa **mirar qué programas están abiertos** y cuáles son procesos del sistema.
    
-   Los atacantes lo usan para:
    
    -   detectar antivirus/EDR y evitarlos,
        
    -   localizar procesos interesantes (navegador, correo, contraseña),
        
    -   elegir un proceso “bueno” donde esconderse (inyección).
        

**Cómo afecta a un ordenador (impacto)**

-   Normalmente el usuario no lo nota directamente.
    
-   Pero si luego se usa para inyección o evasión, puede terminar en:
    
    -   programas que se cierran,
        
    -   fallos extraños,
        
    -   comportamientos “fantasma” (se abre/cierra algo sin razón aparente).
        

**Señales típicas / defensas**

-   Señales: procesos consultando muchos detalles del sistema sin motivo.
    
-   Defensa: EDR y control de acceso a información sensible de procesos.
    

---

#### `IF_ROOT_PRIVS` (`if_admin`)

**En el simulador**

-   Abre un ámbito “si tengo permisos de administrador/root”.
    
-   Permite que acciones “muy potentes” funcionen; si no, el simulador las bloquea y penaliza.
    

**En la vida real**

-   “Administrador/root” significa tener **la llave maestra** del ordenador.
    
-   Con esos permisos se puede:
    
    -   cambiar configuraciones críticas,
        
    -   instalar servicios persistentes,
        
    -   borrar registros,
        
    -   tocar partes del disco que un usuario normal no puede.
        

**Cómo afecta a un ordenador (impacto)**

-   Con privilegios altos, el daño potencial sube muchísimo:
    
    -   persistencia más difícil de quitar,
        
    -   borrado de huellas,
        
    -   impacto en el arranque (boot) o en servicios del sistema.
        

**Señales típicas / defensas**

-   Defensa principal: **mínimos privilegios** (no usar admin para el día a día), UAC/controles, MFA, parches.
    

---

#### `IF_WAN_UP` (`check_internet`)

**En el simulador**

-   Abre un ámbito “si hay internet”.
    
-   Si colocas acciones de red dentro, el motor lo considera más “lógico” (mejor sinergia).
    

**En la vida real**

-   Representa comprobar si el equipo tiene salida a internet antes de:
    
    -   contactar con un servidor remoto (C2),
        
    -   descargar componentes,
        
    -   o enviar datos robados.
        

**Cómo afecta a un ordenador (impacto)**

-   Suele ser “silencioso”, pero es el paso que permite:
    
    -   control remoto,
        
    -   robo de datos,
        
    -   o más infección (descargar “fase 2”).
        

**Señales típicas / defensas**

-   Señales: conexiones recurrentes a dominios raros o patrones de “beaconing” (pulsos).
    
-   Defensa: filtrado DNS/HTTP, proxy, listas de bloqueo, análisis de tráfico.
    

---

#### `TRY_CATCH_ERR` (`try_catch`)

**En el simulador**

-   Abre un ámbito “manejo de errores”.
    
-   Si haces acciones con “poder” dentro, el motor añade complejidad/sinergia (simula robustez).
    

**En la vida real**

-   Es una práctica de programación: capturar fallos para que el programa no se caiga.
    
-   En software malicioso real se usa para:
    
    -   seguir funcionando aunque algo falle,
        
    -   adaptarse si una ruta no existe,
        
    -   o evitar “pantallazos” que alerten al usuario.
        

**Cómo afecta a un ordenador (impacto)**

-   Hace que la actividad sea más persistente y menos visible:
    
    -   menos errores,
        
    -   menos pistas evidentes,
        
    -   más continuidad.
        

**Señales típicas / defensas**

-   Difícil de detectar solo por esto.
    
-   Se detecta por el comportamiento final (persistencia, red, archivos, etc.).
    

---

#### `SYS_SLEEP` (`sleep_timer`)

**En el simulador**

-   Añade una “espera” que reduce ruido en tu puntuación (en el juego es más “sigiloso”).
    

**En la vida real**

-   Representa **retrasar acciones** para:
    
    -   evitar análisis automático (sandboxes que miran pocos segundos),
        
    -   esperar que el usuario se conecte o abra algo,
        
    -   o “enfriar” la actividad para no levantar sospechas.
        

**Cómo afecta a un ordenador (impacto)**

-   El usuario puede notar “nada” al principio y el problema aparece más tarde.
    
-   Esto complica la investigación porque el incidente no coincide con “el momento de abrir el archivo”.
    

**Señales típicas / defensas**

-   Defensa: monitorización por comportamiento a largo plazo, no solo en el primer minuto.
    

---

### NET (conectividad, exploración, control remoto y movimiento lateral)

#### `BIND_C2_SHELL` (`c2_connect`)

**En el simulador**

-   Simula “conectar a un servidor de mando y control (C2)”.
    
-   Si lo haces dentro de `IF_WAN_UP`, el motor lo premia por coherencia.
    

**En la vida real**

-   C2 significa que un atacante crea un “canal” para:
    
    -   enviar órdenes al equipo infectado,
        
    -   recibir información,
        
    -   actualizar el malware,
        
    -   mantener control remoto.
        

**Cómo afecta a un ordenador (impacto)**

-   Riesgos típicos:
    
    -   robo continuo de datos,
        
    -   instalación de más herramientas,
        
    -   uso del equipo como “puente” a otros equipos.
        

**Señales típicas / defensas**

-   Señales: conexiones periódicas a destinos raros, patrones repetidos, tráfico cifrado extraño.
    
-   Defensa: filtrado de salida, inspección de red, EDR, bloquear dominios maliciosos, segmentación.
    

---

#### `SMB_PROPAGATE` (`smb_spread`)

**En el simulador**

-   Representa propagación en red “tipo gusano” por recursos compartidos.
    
-   Solo “funciona” si está dentro de `FOR_EACH_IP` (necesita una lista de objetivos).
    

**En la vida real**

-   SMB es un protocolo de compartición de archivos/recursos en redes (muy común en entornos Windows).
    
-   En ataques reales se usa para:
    
    -   moverse a otros equipos con credenciales válidas,
        
    -   copiar herramientas,
        
    -   ejecutar acciones en máquinas de la red.
        

**Cómo afecta a un ordenador (impacto)**

-   A nivel víctima/empresa puede producir:
    
    -   infección de muchos equipos en poco tiempo,
        
    -   saturación de red,
        
    -   caída de servicios si se combina con ransomware/wiper.
        

**Señales típicas / defensas**

-   Defensa: limitar accesos laterales, segmentación, MFA, contraseñas fuertes, mínimos privilegios, monitorizar accesos a recursos compartidos.
    

---

#### `DNS_EXFILTRATE` (`dns_tunnel`)

**En el simulador**

-   Simula “enviar datos usando DNS” con poco ruido (parece tráfico normal).
    

**En la vida real**

-   DNS es el “sistema de nombres” de internet (traduce nombres a direcciones).
    
-   Algunos ataques intentan “esconder” información dentro de consultas/respuestas DNS:
    
    -   como si metieras mensajes dentro de sobres que normalmente llevan direcciones.
        
-   Se usa para:
    
    -   mandar órdenes (comandos) discretamente,
        
    -   o sacar datos de una red que bloquea otros protocolos.
        

**Cómo afecta a un ordenador (impacto)**

-   Puede saltarse controles si una red deja DNS muy libre.
    
-   Puede filtrar pequeñas cantidades de datos durante mucho tiempo sin ser evidente.
    

**Señales típicas / defensas**

-   Señales: consultas DNS raras (muy largas, muy frecuentes o con patrones extraños).
    
-   Defensa: DNS seguro/filtrado, análisis de logs DNS, bloquear resoluciones anómalas.
    

---

#### `SYN_PORT_SCAN` (`port_scan`)

**En el simulador**

-   Simula escaneo ruidoso de puertos (sube mucho el “ruido”).
    

**En la vida real**

-   Un “puerto” es como una puerta numerada por la que un servicio escucha (web, correo, etc.).
    
-   Un escaneo intenta descubrir:
    
    -   qué puertas están abiertas,
        
    -   qué servicios hay,
        
    -   qué podría ser vulnerable.
        

**Cómo afecta a un ordenador (impacto)**

-   Puede causar:
    
    -   muchas conexiones fallidas,
        
    -   registros (logs) llenos,
        
    -   alertas en firewall/IDS,
        
    -   ligera carga extra (especialmente si es masivo).
        

**Señales típicas / defensas**

-   Defensa: firewalls, IDS/IPS, alertas por patrones de escaneo, limitar exposición de servicios.
    

---

#### `FETCH_PAYLOAD` (`download_stage2`)

**En el simulador**

-   Simula “descargar una segunda fase” (stage2).
    
-   Si lo pones tras “hay internet”, encaja mejor en lógica.
    

**En la vida real**

-   Muchos ataques entran con algo pequeño (primer archivo) y luego descargan:
    
    -   herramientas adicionales,
        
    -   módulos de robo de datos,
        
    -   ransomware,
        
    -   o utilidades para moverse por la red.
        

**Cómo afecta a un ordenador (impacto)**

-   Aumenta la peligrosidad porque “lo peor” llega después:
    
    -   el primer archivo puede parecer inofensivo,
        
    -   pero luego se amplía el ataque.
        

**Señales típicas / defensas**

-   Defensa: bloqueo de descargas sospechosas, permitir solo software firmado/permitido, proxy, análisis de tráfico, EDR.
    

---

### PERSISTENCE (cómo un atacante intenta “quedarse” incluso si reinicias)

> Idea clave: persistencia = “poner una alarma interna” para que algo se ejecute solo en el futuro:  
> al arrancar, al iniciar sesión, cada X minutos, o cuando pase un evento.

#### `SC_CREATE_DAEMON` (`service_create`)

**En el simulador**

-   Bloque de persistencia potente; requiere `IF_ROOT_PRIVS` para funcionar (como en la vida real).
    

**En la vida real**

-   Un “servicio” es un programa que puede arrancar con el sistema y correr en segundo plano.
    
-   Un atacante puede intentar:
    
    -   crear un servicio nuevo,
        
    -   o modificar uno existente,  
        para que ejecute su programa automáticamente.
        

**Cómo afecta a un ordenador (impacto)**

-   El malware vuelve tras reinicio.
    
-   Puede ejecutarse con permisos altos y ser difícil de detectar para un usuario normal.
    

**Señales típicas / defensas**

-   Señales: servicios nuevos o cambiados sin motivo.
    
-   Defensa: control de cambios, EDR, políticas de instalación, auditoría y mínimos privilegios.
    

---

#### `REGISTRY_RUN_KEY` (`reg_run`)

**En el simulador**

-   Persistencia “clásica” de Windows: ejecutarse al iniciar sesión.
    
-   No requiere root en todos los casos, pero puede depender de dónde se escriba.
    

**En la vida real**

-   Windows tiene mecanismos para “programas de inicio”.
    
-   Un atacante puede añadir una referencia para que su programa se ejecute cuando el usuario entra.
    

**Cómo afecta a un ordenador (impacto)**

-   Vuelve a activarse cada vez que el usuario inicia sesión.
    
-   Puede causar:
    
    -   lentitud al arrancar,
        
    -   ventanas raras,
        
    -   o actividad de red nada más iniciar.
        

**Señales típicas / defensas**

-   Defensa: revisar elementos de inicio, EDR, listas de permitidos, controlar cambios en autoinicio.
    

---

#### `CRON_SCHTASKS` (`schtasks`)

**En el simulador**

-   Simula “programar una tarea” para ejecutarse sola.
    
-   Aporta persistencia y también puede servir para “reintentos” periódicos.
    

**En la vida real**

-   Tanto Windows como sistemas tipo Linux/macOS tienen planificadores:
    
    -   ejecutar cosas a una hora,
        
    -   cada X minutos,
        
    -   al arrancar.
        
-   Un atacante lo usa para que el malware se relance aunque lo cierres.
    

**Cómo afecta a un ordenador (impacto)**

-   Síntomas:
    
    -   actividad que reaparece cada cierto tiempo,
        
    -   picos de CPU o red “a la misma hora”,
        
    -   reaparición tras reinicio.
        

**Señales típicas / defensas**

-   Defensa: auditoría de tareas programadas, control de integridad, EDR.
    

---

#### `WMI_SUBSCRIPTION` (`wmi_persist`)

**En el simulador**

-   Persistencia avanzada; requiere `IF_ROOT_PRIVS`.
    

**En la vida real**

-   WMI permite automatizaciones y eventos del sistema.
    
-   Un atacante puede crear una “suscripción” para que cuando pase algo (por ejemplo, inicio de sesión o un evento),  
    se ejecute su código.
    
-   Es famosa porque puede ser poco visible para usuarios.
    

**Cómo afecta a un ordenador (impacto)**

-   Persistencia difícil de detectar “a simple vista”.
    
-   Puede ejecutarse con permisos altos dependiendo del contexto.
    

**Señales típicas / defensas**

-   Defensa: reglas EDR específicas, auditoría de WMI, control de cambios, monitorización de eventos.
    

---

#### `ADD_GHOST_USER` (`account_create`)

**En el simulador**

-   Simula crear un “usuario fantasma” para volver a entrar más tarde.
    

**En la vida real**

-   Un atacante puede crear cuentas nuevas (locales, de dominio o en la nube) para:
    
    -   mantener acceso aunque se limpie el malware,
        
    -   tener una puerta trasera “legítima” con contraseña.
        
-   A veces también manipulan cuentas existentes (añadir permisos, cambiar contraseñas, etc.).
    

**Cómo afecta a un ordenador (impacto)**

-   Riesgo muy alto: incluso si “parece todo limpio”, la cuenta puede permitir volver a entrar.
    
-   Puede afectar a toda una organización si la cuenta es de dominio.
    

**Señales típicas / defensas**

-   Defensa: alertas por creación de cuentas, revisiones periódicas, MFA, mínimo privilegio, inventario de cuentas.
    

---

#### `BITSADMIN_JOB` (`bits_job`)

**En el simulador**

-   Persistencia/descarga discreta “aprovechando un servicio del sistema”.
    

**En la vida real**

-   BITS es un componente legítimo de Windows para transferir archivos en segundo plano.
    
-   Un atacante puede abusarlo para:
    
    -   descargar (o subir) archivos sin “molestar” al usuario,
        
    -   reanudar transferencias tras reinicio,
        
    -   camuflarse como tráfico normal.
        

**Cómo afecta a un ordenador (impacto)**

-   Puede “colar” descargas sin que el usuario vea una ventana.
    
-   Puede mantener transferencias durante horas/días sin llamar la atención.
    

**Señales típicas / defensas**

-   Defensa: monitorizar uso anómalo de BITS, listas de permitidos, EDR y registro de transferencias.
    

---

### PAYLOAD (lo que “hace el daño” o el objetivo final)

#### `CRYPTO_AES256` (`encrypt_aes`)

**En el simulador**

-   Si está dentro de `FOR_EACH_FILE`, se vuelve extremadamente potente (cifrado masivo).
    
-   Fuera del bucle, su efecto es pequeño (cifrar “una cosa” no tiene el mismo impacto).
    

**En la vida real**

-   AES es un algoritmo de cifrado legítimo (se usa para proteger datos).
    
-   En ransomware, el atacante cifra archivos del usuario para volverlos inaccesibles,  
    y pide dinero por la clave (o para “desbloquear”).
    
-   Es “impactante” porque rompe la disponibilidad: no puedes abrir tus propios archivos.
    

**Cómo afecta a un ordenador (impacto)**

-   Consecuencias típicas:
    
    -   documentos inutilizables,
        
    -   pérdida de productividad,
        
    -   parón de empresa,
        
    -   restauración desde copias de seguridad (si existen).
        

**Señales típicas / defensas**

-   Señales: modificaciones masivas de archivos, patrones de cifrado, notas de rescate.
    
-   Defensa: backups, segmentación, mínimo privilegio, EDR con detección de cifrado masivo.
    

---

#### `MBR_WIPER` (`wiper`)

**En el simulador**

-   Acción “destructiva” que requiere `IF_ROOT_PRIVS`.
    

**En la vida real**

-   Un “wiper” busca destruir datos de forma permanente.
    
-   Si ataca estructuras críticas del disco (como el arranque), el PC puede:
    
    -   no iniciar,
        
    -   mostrar errores al arrancar,
        
    -   perder acceso a particiones.
        
-   Es más “destrucción” que “extorsión”.
    

**Cómo afecta a un ordenador (impacto)**

-   Puede dejar equipos inutilizables.
    
-   Recuperación compleja/costosa: reinstalación, restauración, a veces pérdida total.
    

**Señales típicas / defensas**

-   Defensa: protección de arranque, controles de escritura a disco, EDR, copias de seguridad offline, respuesta rápida.
    

---

#### `DATA_UPLOAD` (`exfil_data`)

**En el simulador**

-   Si está dentro de `FOR_EACH_FILE`, simula robar muchos archivos (muy potente).
    
-   Fuera del bucle, simula una filtración pequeña.
    

**En la vida real**

-   “Exfiltración” es sacar datos fuera: subirlos a internet.
    
-   Puede ser:
    
    -   robo de documentos,
        
    -   datos personales,
        
    -   credenciales,
        
    -   propiedad intelectual.
        
-   A menudo se hace por canales que parezcan normales (HTTPS, nube, etc.).
    

**Cómo afecta a un ordenador (impacto)**

-   A la víctima puede no “romperle” nada… pero el daño es:
    
    -   fuga de información,
        
    -   multas/regulación,
        
    -   chantaje,
        
    -   pérdida reputacional.
        

**Señales típicas / defensas**

-   Señales: subida de datos anómala, conexiones a servicios no habituales, picos de tráfico saliente.
    
-   Defensa: DLP, monitorización de red, control de acceso a datos, segmentación, EDR.
    

---

#### `HOOK_KEYBOARD` (`keylogger`)

**En el simulador**

-   Simula capturar teclas.
    
-   Si está dentro de `OBFUSCATE_XOR`, el motor lo hace más “efectivo” (más difícil de detectar).
    

**En la vida real**

-   Un keylogger registra lo que tecleas para robar:
    
    -   contraseñas,
        
    -   números de tarjeta,
        
    -   mensajes,
        
    -   información sensible.
        
-   Puede hacerlo de forma invisible para el usuario.
    

**Cómo afecta a un ordenador (impacto)**

-   No suele ralentizar mucho, por eso es peligroso: puede pasar desapercibido.
    
-   El impacto real es el robo de cuentas (correo, banco, empresa).
    

**Señales típicas / defensas**

-   Defensa: MFA, EDR, protección del navegador, revisar accesos sospechosos, higiene de contraseñas.
    

---

#### `FRAME_GRABBER` (`screen_capture`)

**En el simulador**

-   Simula capturar pantallas (screenshots).
    
-   Con ofuscación, sube su efectividad.
    

**En la vida real**

-   Capturar pantalla sirve para robar:
    
    -   lo que ves (documentos, chats, paneles),
        
    -   códigos temporales,
        
    -   datos que no están en archivos (solo visibles en pantalla).
        
-   Se usa en espionaje y en intrusiones prolongadas.
    

**Cómo afecta a un ordenador (impacto)**

-   Puede no notarse.
    
-   El daño es privacidad y filtración de información: “te miran por encima del hombro”.
    

**Señales típicas / defensas**

-   Defensa: EDR, permisos restringidos, detección de herramientas de acceso remoto, segmentación, MFA.
    

---

### EVASION (ocultar, confundir, sobrevivir a análisis y borrar huellas)

#### `OBFUSCATE_XOR` (`obfuscate`)

**En el simulador**

-   Reduce “ruido” de los bloques dentro.
    
-   Es como “envolver” acciones para que parezcan menos obvias.
    

**En la vida real**

-   Ofuscación = hacer que un archivo o código sea difícil de analizar:
    
    -   cifrar/mezclar contenido,
        
    -   esconder cadenas,
        
    -   complicar lectura.
        
-   El objetivo es evitar antivirus y analistas.
    

**Cómo afecta a un ordenador (impacto)**

-   No daña por sí mismo; sirve para que otras acciones dañinas duren más tiempo sin ser detectadas.
    

**Señales típicas / defensas**

-   Defensa: análisis de comportamiento, sandboxing avanzado, detección de empaquetadores/ofuscación, inteligencia de amenazas.
    

---

#### `PROC_HOLLOWING` (`process_inject`)

**En el simulador**

-   Simula “meterse dentro” de otro proceso para camuflarse.
    

**En la vida real**

-   Process hollowing / inyección: el atacante intenta ejecutar su código “dentro” de un proceso legítimo.
    
-   Beneficios para el atacante:
    
    -   parece que el proceso bueno es el que hace cosas malas,
        
    -   evade controles basados en “qué programa es”.
        

**Cómo afecta a un ordenador (impacto)**

-   Puede causar:
    
    -   inestabilidad (cuelgues),
        
    -   comportamientos raros en programas legítimos,
        
    -   actividad de red desde procesos inesperados.
        

**Señales típicas / defensas**

-   Defensa: EDR con detección de inyección, reglas de integridad, aislamiento de procesos, control de aplicaciones.
    

---

#### `MACE_TIMESTOMP` (`timestomp`)

**En el simulador**

-   Simula cambiar “fechas” de archivos para ocultar actividad.
    

**En la vida real**

-   Timestomp = modificar marcas de tiempo (creación/modificación/acceso) para:
    
    -   hacer que un archivo malicioso parezca antiguo,
        
    -   confundir investigaciones,
        
    -   “mezclarse” con archivos legítimos.
        

**Cómo afecta a un ordenador (impacto)**

-   No rompe nada del usuario directamente.
    
-   Dificulta a los defensores reconstruir qué pasó y cuándo.
    

**Señales típicas / defensas**

-   Defensa: registros centralizados, integridad de archivos, correlación de eventos (no confiar solo en timestamps).
    

---

#### `CLEAR_EVENTLOGS` (`delete_logs`)

**En el simulador**

-   Requiere `IF_ROOT_PRIVS`. Si no, falla y “hace ruido”.
    

**En la vida real**

-   Borrar registros del sistema es una técnica para ocultar rastro:
    
    -   elimina o reduce evidencias de accesos, fallos, conexiones.
        
-   Es una parte típica de “anti-forense”.
    

**Cómo afecta a un ordenador (impacto)**

-   El usuario normal no lo nota.
    
-   Pero la organización pierde visibilidad: investigar se vuelve mucho más difícil.
    

**Señales típicas / defensas**

-   Defensa: enviar logs a un servidor externo (SIEM), alertas de borrado, control de privilegios.
    

---

#### `DLL_SEARCH_ORDER` (`dll_hijack`)

**En el simulador**

-   Simula secuestrar cómo un programa carga “librerías” (piezas de software compartidas).
    

**En la vida real**

-   Muchos programas cargan DLLs (bibliotecas) al arrancar.
    
-   Si el sistema busca una DLL en varias ubicaciones y un atacante coloca una DLL falsa donde se busque antes,  
    el programa puede cargar la DLL maliciosa sin que el usuario lo sepa.
    
-   Se usa para:
    
    -   persistencia,
        
    -   ejecución,
        
    -   evasión.
        

**Cómo afecta a un ordenador (impacto)**

-   Puede provocar:
    
    -   que un programa legítimo haga cosas maliciosas,
        
    -   fallos si la DLL no encaja bien,
        
    -   actividad anómala al abrir un programa normal.
        

**Señales típicas / defensas**

-   Defensa: rutas de carga seguras, control de integridad, firmas, EDR, hardening de aplicaciones.
    

---
