import subprocess

def check_security_updates():
    # Perbarui daftar paket
    subprocess.run(['sudo', 'apt', 'update'])
    # Dapatkan daftar pembaruan keamanan yang tersedia
    result = subprocess.run(
        ['apt', 'list', '--upgradable'],
        capture_output=True, text=True
    )
    lines = result.stdout.splitlines()
    security_updates = []
    for line in lines:
        if 'security' in line:
            security_updates.append(line)
    return security_updates

def install_security_updates(security_updates):
    for update in security_updates:
        package = update.split('/')[0]
        print(f"Installing security update: {package}")
        subprocess.run(['sudo', 'apt-get', 'install', '--only-upgrade', package, '-y'])

if __name__ == '__main__':
    updates = check_security_updates()
    if updates:
        print("Security updates available:")
        for u in updates:
            print(u)
        install = input("Install all security updates? (y/n): ")
        if install.lower() == 'y':
            install_security_updates(updates)
    else:
        print("No security updates available.")
