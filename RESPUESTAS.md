# RESPUESTAS — Reflexión y Resumen (Plantilla genérica)

> **Actividad / ID:** Implementación de SIEM con ELK y Suricata
> **Unidad / Tema:** Sistemas de Detección de Intrusos (IDS) y SIEM
> **Alumno/a:** Javier Calvillo Acebedo
> **Fecha:** 23 de febrero de 2026

---

## 1) Reflexión crítica (preguntas)

### 1.1) ¿Qué te han parecido los temas tratados en la unidad?
- Me han parecido muy prácticos y reveladores. Te ayudan a ver la "imagen completa" de la ciberseguridad. Normalmente aprendemos a atacar o a proteger una cosa concreta, pero aquí hemos visto todo el viaje: desde que el atacante lanza el primer ping, cómo el sistema lo detecta, cómo se guarda ese aviso y cómo llega finalmente a una pantalla gráfica para que un analista lo vea. Le da mucho sentido a toda la teoría.

### 1.2) ¿Qué ha sido más útil para tu futuro puesto de trabajo? ¿Por qué?
- Aprender a solucionar problemas reales (troubleshooting) cuando las cosas no funcionan a la primera. En el mundo real, los programas fallan o las configuraciones chocan entre sí. Habernos peleado con Docker, con interfaces de red que no escuchaban donde debían, o descubrir que había que borrar un archivo viejo (PID) para que Suricata arrancara, me ha dado mucha más experiencia que si todo hubiera funcionado pulsando un solo botón. 

### 1.3) ¿Qué partes ya conocías y cuáles han sido nuevas para ti?
- **Ya conocía:** El uso básico de máquinas virtuales, comandos sencillos de Linux, cómo hacer un ping para ver si hay conexión, y la idea general de lo que es un ataque por fuerza bruta (SSH).
- **Nuevas para mí:** Instalar y configurar un IDS como Suricata desde cero, escribir mis propias reglas de seguridad, y sobre todo, montar la conexión para que esos avisos viajen automáticamente a un panel visual (Kibana) usando Filebeat.

### 1.4) ¿Qué concepto/idea te ha llamado más la atención y por qué?
- La idea de poner "límites" a las alertas (usando la opción `threshold` en las reglas). Si alguien ataca por fuerza bruta, puede hacer miles de intentos en un minuto. Si el sistema nos manda una alerta por cada intento, nos volveríamos locos y el sistema colapsaría. Decirle al programa "avísame solo si hace 5 intentos en un minuto" me pareció una forma muy inteligente y lógica de evitar el exceso de ruido.

### 1.5) ¿Qué parte recortarías o simplificarías si hubiera menos tiempo? Justifica.
- Simplificaría la instalación manual de programas básicos dentro de la máquina víctima (como tener que instalar el ping, el cliente SSH o actualizar el sistema). Si tuviéramos menos tiempo, usar una máquina que ya traiga todo eso instalado de fábrica nos permitiría centrarnos al 100% en lo verdaderamente importante: crear las reglas de seguridad y analizar los ataques en el panel.

### 1.6) ¿Qué tema has echado en falta o ampliarías? Justifica.
- Me habría gustado ver cómo hacer que el sistema bloquee el ataque automáticamente. En esta práctica hemos logrado detectar al atacante y ver sus pasos en una pantalla (modo IDS), pero el atacante podía seguir intentando entrar. Sería genial aprender a conectar este aviso con un cortafuegos para que, en cuanto detecte la fuerza bruta, bloquee la IP del atacante al instante.

### 1.7) ¿Qué aplicarías “mañana” en un entorno real con recursos limitados?
- La idea de centralizar los avisos de seguridad. Aunque una empresa pequeña no tenga dinero para montar un servidor ELK gigante, instalar un programa pequeño como Filebeat en sus servidores para que envíe los registros de errores a un solo ordenador central es básico. No puedes proteger lo que no ves, y tener que entrar servidor por servidor a leer archivos de texto no es viable.

### 1.8) ¿Qué duda, riesgo o punto crítico te queda abierto tras la unidad?
- Me pregunto qué pasa si el atacante logra entrar a la máquina. Al tener el programa de seguridad (Suricata y Filebeat) instalado en el mismo ordenador que está siendo atacado, ¿no podría el atacante simplemente apagar esos programas en cuanto entre para que dejemos de ver lo que hace? Aislar bien las herramientas de seguridad me parece un reto complicado.


## 2) Resumen esquematizado (obligatorio)

En esta unidad hemos construido un sistema completo de vigilancia y alerta siguiendo estos pasos y componentes:

1. **La Infraestructura (Docker):** Creamos una red virtual aislada para que nuestras máquinas se comuniquen de forma segura sin pisar otras redes.
2. **El Cerebro (ELK Stack):** Desplegamos un servidor central que recibe datos (Logstash), los almacena ordenados (Elasticsearch) y nos los muestra con gráficos bonitos (Kibana).
3. **El Vigilante (Suricata):** Instalamos un Detector de Intrusos (IDS) en la máquina víctima. Le enseñamos a detectar dos cosas mediante reglas:
   - *Regla 1:* Detectar cuando alguien hace un barrido de red (Ping/ICMP).
   - *Regla 2:* Detectar cuando alguien intenta adivinar contraseñas (Fuerza Bruta SSH).
4. **El Mensajero (Filebeat):** Configuramos este pequeño programa para que lea la libreta de apuntes del vigilante (`fast.log`) y la envíe rápidamente al cerebro (ELK).
5. **El Ataque y la Visualización:** Simulamos ser los malos atacando nuestro propio sistema (usando Hydra y comandos Bash) y luego fuimos al panel de Kibana para comprobar que nuestro sistema nos había avisado creando un gráfico circular con los ataques.

### 2.1) Mapa/índice de la unidad (visión global)
- Montaje del entorno base (Redes y contenedores).
- Instalación del sistema de recolección (ELK).
- Configuración de la defensa (Suricata y reglas locales).
- Conexión de los sistemas (Filebeat).
- Pruebas de auditoría (Ataques controlados).
- Creación de Cuadros de Mando (Dashboards en Kibana).

### 2.2) Conceptos clave (lista breve)
- **SIEM:** El panel central donde se junta toda la información de seguridad.
- **IDS:** Un "perro guardián" que avisa si ve tráfico raro, pero no lo muerde (no lo bloquea).
- **Log:** El registro o archivo de texto donde se apuntan los eventos.
- **Fuerza Bruta:** Intentar entrar a un sistema probando miles de contraseñas.

### 2.3) Procesos / procedimientos (pasos o checklist)
- [x] Crear la red y lanzar el contenedor de ELK.
- [x] Lanzar la máquina víctima y prepararla.
- [x] Instalar Suricata y escribir las reglas para ICMP y SSH.
- [x] Solucionar errores de arranque del IDS (interfaz, PID, modo de red).
- [x] Configurar Filebeat para enviar el archivo `fast.log` al servidor.
- [x] Atacar el servidor para generar datos reales.
- [x] Entrar a Kibana, buscar los datos y hacer un gráfico circular.

### 2.4) Herramientas / técnicas (si aplica)
- **Para montar todo:** Docker.
- **Para proteger y analizar:** Suricata (IDS), Filebeat, Elasticsearch, Kibana.
- **Para atacar (simulación):** Kali Linux, herramienta Hydra, comandos de consola (Bash).

### 2.5) Buenas prácticas y errores típicos
- **Buena práctica:** Poner límites (threshold) a las reglas para no inundar el sistema con alertas repetidas.
- **Error típico:** Intentar arrancar un servicio en Docker sin comprobar si se cerró mal la vez anterior. A veces se queda un archivo "basura" (PID) que hay que borrar a mano para que vuelva a funcionar.
- **Error típico:** Olvidar que el tráfico local de una misma máquina va por una interfaz invisible llamada "loopback" (`lo`) y no por la tarjeta de red normal.

### 2.6) Glosario mínimo (términos y definiciones cortas)
- **Kibana:** La página web gráfica donde vemos los gráficos y las alertas.
- **Elasticsearch:** La base de datos súper rápida que guarda las alertas.
- **Regla (Rule):** Una frase de código que le dice al IDS qué aspecto tiene un ataque.
- **Loopback (lo):** La dirección IP que usa el ordenador para hablar consigo mismo (127.0.0.1).


## 3) (Opcional) Evidencias y recursos usados

### Evidencia 1
- **Archivo:** `evidencias/Captura 32.png`
- **Qué demuestra:** Muestra la pantalla negra de la consola donde realizamos el ataque de fuerza bruta por SSH y cómo, justo después, al abrir el archivo de registros (`fast.log`), aparecen perfectamente escritas las alertas generadas por nuestro IDS.
- **Qué he aprendido:** Que a veces la interfaz gráfica no lo es todo. Saber ir a los archivos de texto originales del sistema para confirmar que todo funciona antes de enviarlo al servidor central te ahorra muchos dolores de cabeza.

### Evidencia 2
- **Archivo:** `evidencias/Captura 33.png`
- **Qué demuestra:** La llegada de los datos al sistema SIEM y su visualización en Kibana. Se ve cómo el sistema es capaz de separar los ataques de Ping de los ataques de SSH.
- **Qué he aprendido:** A transformar letras y números aburridos en gráficos visuales útiles. Entendí que en el trabajo real nadie lee archivos de texto línea por línea; se usan estos paneles para ver de un vistazo si la empresa está bajo ataque.


## 4) Conclusión (cierre)
Esta actividad ha sido la demostración práctica de que la ciberseguridad es un trabajo de equipo entre varias herramientas. No sirve de nada tener un IDS muy bueno detectando cosas si luego nadie lee sus alertas, y tampoco sirve de nada tener un panel de gráficos muy bonito si no sabemos configurarlo para que reciba datos. Haber conseguido conectar un ataque real, pasando por un detector, un mensajero de archivos y una base de datos, hasta verlo en un gráfico en pantalla, me ha hecho entender realmente cómo funciona el departamento de seguridad (SOC) de una empresa por dentro.
