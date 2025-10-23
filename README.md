# Day-z-Error-0x00020009-Solution
Day Z connection error definitive solution made by mfxi
import os
import subprocess
import shutil
import sys
import time

def run_command(cmd):
    try:
        subprocess.run(cmd, check=True, shell=True)
    except subprocess.CalledProcessError as e:
        print(f"Error ejecutando: {cmd}\n{e}")

def verificar_ruta_dayz():
    steam_path = r"C:\Program Files (x86)\Steam\steamapps\common\DayZ"
    if not os.path.exists(steam_path):
        print("❌ No se encontró la carpeta de DayZ en la ruta estándar.")
        print("Por favor revisa si lo tienes instalado en otra unidad.")
        return None
    print("✅ Carpeta de DayZ encontrada.")
    return steam_path

def limpiar_battleye(dayz_path):
    battleye_path = os.path.join(dayz_path, "BattlEye")
    if os.path.exists(battleye_path):
        shutil.rmtree(battleye_path, ignore_errors=True)
        print("🧹 Caché de BattlEye eliminada.")
    else:
        print("No se encontró carpeta BattlEye.")

def verificar_integridad():
    print("🔍 Verificando integridad de archivos de DayZ con SteamCMD...")
    cmd = '"C:\\Program Files (x86)\\Steam\\steam.exe" -command "validate"'
    run_command(cmd)

def reinstalar_vcredist():
    urls = [
        "https://aka.ms/vs/17/release/vc_redist.x64.exe",
        "https://aka.ms/vs/17/release/vc_redist.x86.exe"
    ]
    print("⬇️ Descargando e instalando Visual C++ redistributables...")
    for url in urls:
        run_command(f"start {url}")
    print("👉 Instala ambos paquetes manualmente cuando se abran.")

def main():
    print("=== Reparador automático de DayZ ===")
    dayz_path = verificar_ruta_dayz()
    if not dayz_path:
        sys.exit(1)
    
    limpiar_battleye(dayz_path)
    verificar_integridad()
    reinstalar_vcredist()
    print("\n✅ Listo. Reinicia tu PC y prueba abrir DayZ desde Steam.")

if __name__ == "__main__":
    if not os.name == "nt":
        print("Este script es solo para Windows.")
        sys.exit(1)
    main()
    time.sleep(2)
