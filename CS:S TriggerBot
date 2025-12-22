import time
import ctypes
import pymem
import pymem.process

# process / module
proc_name = "cstrike_win64.exe"
dll_name  = "client.dll"

# offsets in use
offs = {
    "lp":    0x0068EEC8,  # local player
    "ents":  0x006098C8,  # entity list
    "team":  0xD8,        # team (2=T, 3=CT)
    "step":  0x20,        # entity list stride
    "cross": 0x00649910,  # base for crosshair_id chain
}

cooldown = 0.08  # minimal interval between clicks

# simple left-click (down/up)
u32 = ctypes.windll.user32
def tap():
    u32.mouse_event(0x0002, 0, 0, 0, 0)
    time.sleep(0.008)
    u32.mouse_event(0x0004, 0, 0, 0, 0)

# attach and resolve client base
pm = pymem.Pymem(proc_name)
client_base = pymem.process.module_from_name(pm.process_handle, dll_name).lpBaseOfDll

# note: if your binary is 32-bit, switch read_ulonglong -> read_uint here
def rptr(addr):
    return pm.read_ulonglong(addr)

print("[+] trigger (team-based)")

last = 0.0
while True:
    try:
        # local player
        me = rptr(client_base + offs["lp"])
        if not me:
            time.sleep(0.001)
            continue

        my_team = pm.read_int(me + offs["team"])

        # crosshair id (short chain)
        node = rptr(client_base + offs["cross"])
        node = rptr(node + 0x18)
        node = rptr(node + 0x10)
        cross_id = pm.read_int(node + 0x50)
        print(f"id={cross_id:>3}", end="\r")

        if cross_id <= 0:
            time.sleep(0.001)
            continue

        idx = cross_id - 1
        ent_slot = (client_base + offs["ents"]) + (idx * offs["step"])
        target = rptr(ent_slot)
        if not target:
            time.sleep(0.001)
            continue

        enemy_team = pm.read_int(target + offs["team"])

        if enemy_team != my_team:
            now = time.time()
            if now - last >= cooldown:
                tap()
                last = now

        time.sleep(0.001)

    except KeyboardInterrupt:
        print("\n[-] stop")
        break
    except Exception as e:
        print(f"\n[err] {e}")
        time.sleep(0.05)
