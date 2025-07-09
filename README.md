# Arch Manual

### Package Manager คืออะไร?

ก่อนอื่น มาทำความเข้าใจกันก่อนว่า Package Manager คืออะไร Package Manager เป็นเครื่องมือที่ใช้สำหรับจัดการซอฟต์แวร์บนระบบปฏิบัติการ Linux ทำหน้าที่หลักๆ ในการ:
- **ติดตั้ง (Install):** ดาวน์โหลดและติดตั้งแพ็คเกจซอฟต์แวร์ที่ต้องการ

- **อัปเดต (Update):** ตรวจสอบและอัปเดตแพ็คเกจซอฟต์แวร์ให้เป็นเวอร์ชันล่าสุด

- **ลบ (Remove/Uninstall):** ลบแพ็คเกจซอฟต์แวร์ออกจากระบบ

- **ค้นหา (Search):** ค้นหาแพ็คเกจซอฟต์แวร์ที่มีอยู่ใน repositories

- **จัดการ dependencies:** ดูแลเรื่อง dependency ของแพ็คเกจต่างๆ (เช่น หากซอฟต์แวร์ A ต้องการซอฟต์แวร์ B ในการทำงาน Package Manager จะติดตั้ง B ให้โดยอัตโนมัติเมื่อเราติดตั้ง A)

### [Pacman: Official Package Manager ของ Arch Linux](https://archlinux.org/packages/)

`pacman` (package manager utility) เป็น Package Manager อย่างเป็นทางการของ Arch Linux มันถูกออกแบบมาให้เรียบง่าย รวดเร็ว และมีประสิทธิภาพ โดยใช้ไฟล์แพ็คเกจแบบบีบอัดที่เรียกว่า `.pkg.tar.zst` (เดิมใช้ `.pkg.tar.xz`)


### หลักการทำงานของ Pacman:

`pacman` ทำงานร่วมกับ `repositories` ซึ่งเป็นแหล่งเก็บแพ็คเกจซอฟต์แวร์อย่างเป็นทางการของ Arch Linux โดย `pacman` จะดาวน์โหลดแพ็คเกจจาก repositories เหล่านี้ และติดตั้งลงบนระบบของคุณ

### คำสั่งของ Pacman

- **การอัปเดตระบบและแพ็คเกจ:**

    ```
    $ sudo pacman -Syu
    ```

    - `-S:` Sync packages (ซิงค์แพ็คเกจจาก repositories)

    - `y:` Refresh local package database (อัปเดตฐานข้อมูลแพ็คเกจในเครื่องให้เป็นเวอร์ชันล่าสุด)

    - `u:` Upgrade installed packages (อัปเกรดแพ็คเกจที่ติดตั้งอยู่ในเครื่อง)
นี่เป็นคำสั่งที่สำคัญที่สุดและควรทำเป็นประจำเพื่อความปลอดภัยและประสิทธิภาพของระบบ

- **การติดตั้งแพ็คเกจ:**
    ```
    $ sudo pacman -S <package_name>
    ```

    **ตัวอย่าง: ติดตั้ง Firefox**
    ```
    $ sudo pacman -S firefox
    ```
    
    **คุณสามารถติดตั้งหลายแพ็คเกจพร้อมกันได้:**
    ```
    $ sudo pacman -S firefox vim htop
    ```

- **การลบแพ็คเกจ:**
    ```
    $ sudo pacman -R <package_name>
    ```

    - `-R:` Remove package (ลบแพ็คเกจ)
ตัวอย่าง: ลบ Firefox
    ```
    $ sudo pacman -R firefox
    ```

    **การลบแพ็คเกจพร้อม dependencies ที่ไม่จำเป็นอีกต่อไป (แนะนำ):**
    ```
    $ sudo pacman -Rs <package_name>
    ```
    - `s:` Remove dependencies (ลบ dependencies ที่ถูกติดตั้งพร้อมกับแพ็คเกจและไม่ถูกใช้โดยแพ็คเกจอื่นอีกต่อไป)

    **การลบแพ็คเกจทั้งหมด รวมถึงไฟล์คอนฟิกูเรชัน (caution):**
    ```
    $ sudo pacman -Rns <package_name>
    ```
    - `n:` Do not save configuration files (จะไม่บันทึกไฟล์คอนฟิกูเรชัน)

    - `s:` Remove dependencies

- **การค้นหาแพ็คเกจ:**
    
    ```
    $ pacman -Ss <keyword>
    ```
    - `-Ss:` Search (ค้นหาแพ็คเกจในฐานข้อมูลของ repositories)
ตัวอย่าง: ค้นหาแพ็คเกจเกี่ยวกับ security
    ```
    $ pacman -Ss security
    ```

    **การดูข้อมูลแพ็คเกจที่ติดตั้งแล้ว:**
    ```
    $ pacman -Qi <package_name>
    ```
    - `-Qi:` Query installed package (ดูข้อมูลของแพ็คเกจที่ติดตั้งแล้ว)

    **การดูข้อมูลแพ็คเกจใน repositories (ที่ยังไม่ได้ติดตั้ง):**

    ```
    $ pacman -Si <package_name>
    ```

    - `-Si:` Query synced package (ดูข้อมูลของแพ็คเกจที่มีอยู่ใน repositories)

- **การลบแพ็คเกจที่ไม่ได้ใช้งาน (orphans):**
บางครั้งเมื่อคุณลบแพ็คเกจ อาจมี dependencies ที่ยังคงค้างอยู่ในระบบและไม่ได้ถูกใช้งานโดยแพ็คเกจอื่น คุณสามารถลบออกได้ด้วยคำสั่ง:

    ```
    $ sudo pacman -Rns $(pacman -Qtdq)
    ```
    - `pacman -Qtdq:` จะแสดงรายชื่อแพ็คเกจที่เป็น orphans

- **การล้าง Package Cache:**
`pacman` จะเก็บไฟล์แพ็คเกจที่ดาวน์โหลดไว้ใน `/var/cache/pacman/pkg/` เพื่อใช้ในการติดตั้งซ้ำหรือดาวน์เกรดในอนาคต แต่ไฟล์เหล่านี้อาจกินพื้นที่ดิสก์ คุณสามารถล้าง cache ได้:
    - **ลบแพ็คเกจเวอร์ชันเก่าทั้งหมด ยกเว้นเวอร์ชันล่าสุดที่ติดตั้งอยู่:**
        ```
        $ sudo pacman -Sc
        ```
    
    - **ลบไฟล์แพ็คเกจทั้งหมดใน cache (ระมัดระวัง):**
        ```
        $ sudo pacman -Scc
        ```
        ไม่แนะนำหากคุณต้องการดาวน์เกรดแพ็คเกจในอนาคต

### [AUR (Arch User Repository)](https://aur.archlinux.org/packages)
AUR เป็นหนึ่งในคุณสมบัติที่โดดเด่นของ Arch Linux เป็น "ที่เก็บแพ็คเกจ" ที่ดูแลโดยผู้ใช้ (community-driven repository) มันไม่ได้เป็นส่วนหนึ่งของ Arch Linux repositories อย่างเป็นทางการ แต่เป็นที่ที่ผู้ใช้สามารถอัปโหลด PKGBUILD ซึ่งเป็นสคริปต์ที่อธิบายวิธีการสร้างแพ็คเกจจากซอร์สโค้ด (หรือบางครั้งก็เป็นไบนารี)

**ความแตกต่างระหว่าง Official Repositories และ AUR:**
| คุณสมบัติ          | Official Repositories (Pacman)          | AUR (Yay/AUR Helpers)                                 |
| :---------------- | :-------------------------------------- | :---------------------------------------------------- |
| **ดูแลโดย** | Arch Linux Developers                   | Arch Linux Community (Users)                          |
| **ความน่าเชื่อถือ** | สูงมาก (แพ็คเกจผ่านการตรวจสอบอย่างเข้มงวด) | มีความหลากหลาย ควรตรวจสอบ PKGBUILD ก่อนติดตั้ง (มีความเสี่ยงเล็กน้อย) |
| **การติดตั้ง** | Pacman (ดาวน์โหลดไบนารีแพ็คเกจที่คอมไพล์แล้ว) | AUR Helpers (ดาวน์โหลด PKGBUILD, คอมไพล์จากซอร์สโค้ด, สร้างแพ็คเกจ, ติดตั้ง) |
| **ประเภทแพ็คเกจ** | ไบนารีแพ็คเกจ (.pkg.tar.zst)           | PKGBUILD (สคริปต์สำหรับสร้างแพ็คเกจ)                     |

**ความสำคัญของ PKGBUILD:**
`PKGBUILD` เป็นไฟล์สคริปต์ Bash ที่ใช้โดย `makepkg` (เครื่องมือที่มาพร้อมกับ pacman) เพื่อสร้างแพ็คเกจ Arch Linux ไฟล์นี้จะระบุ:
- ชื่อแพ็คเกจ
- เวอร์ชัน
- Dependencies
- แหล่งที่มาของซอร์สโค้ด
- ขั้นตอนการคอมไพล์และการติดตั้ง

**ข้อควรระวังในการใช้ AUR:**
เนื่องจากแพ็คเกจใน AUR มาจากชุมชน ไม่ใช่ Arch Linux developers โดยตรง คุณควร ตรวจสอบ PKGBUILD ก่อนการติดตั้งเสมอ เพื่อให้แน่ใจว่าไม่มีโค้ดที่เป็นอันตราย หรือดาวน์โหลดจากแหล่งที่ไม่น่าเชื่อถือ นี่เป็นสิ่งสำคัญอย่างยิ่งสำหรับนักศึกษาด้านความปลอดภัยไซเบอร์

**Yay: AUR Helper ยอดนิยม**
การติดตั้งแพ็คเกจจาก AUR ด้วยมือ (manually) นั้นค่อนข้างซับซ้อน เพราะต้องดาวน์โหลด PKGBUILD, รัน `makepkg -si` เพื่อคอมไพล์และติดตั้ง, และจัดการ dependencies ด้วยตัวเอง

`AUR helpers` เข้ามาช่วยแก้ปัญหานี้ โดยทำหน้าที่เป็น wrapper รอบๆ `pacman` และ `makepkg` ทำให้การติดตั้งและจัดการแพ็คเกจจาก AUR ง่ายขึ้นมาก

`yay` (Yet another Yogurt) เป็น AUR helper ที่ได้รับความนิยมอย่างมาก มันมีความสามารถในการจัดการทั้ง official packages (ผ่าน pacman) และ AUR packages ได้อย่างราบรื่น

**การติดตั้ง Yay:**
เนื่องจาก `yay` อยู่ใน AUR คุณจะต้องติดตั้งด้วยมือครั้งแรก หรือใช้ AUR helper ตัวอื่นที่ติดตั้งไว้แล้ว หากคุณยังไม่มี AUR helper อื่นๆ นี่คือวิธีการติดตั้ง `yay` ด้วยมือ:
1. **ติดตั้ง dependencies ที่จำเป็น:**
    ```
    $ sudo pacman -S --needed base-devel git
    ```
    - `base-devel:` กลุ่มแพ็คเกจที่จำเป็นสำหรับการคอมไพล์ซอฟต์แวร์
    - `git:` สำหรับ clone repository
2. **Clone yay repository:**
    ```
    $ git clone https://aur.archlinux.org/yay.git
    ```

3. **เปลี่ยนไปที่ directory ของ yay:**
    ```
    $ cd yay
    ```

4. **สร้างและติดตั้งแพ็คเกจ:**
    ```
    $ makepkg -si
    ```

    - `makepkg -s:` ดาวน์โหลด dependencies ที่จำเป็นและสร้างแพ็คเกจ
    - `makepkg -i:` ติดตั้งแพ็คเกจที่สร้างขึ้น

**การใช้งาน Yay ที่สำคัญ:**
`yay` มี syntax คล้ายกับ `pacman` มาก ทำให้ง่ายต่อการเรียนรู้

- **อัปเดตระบบทั้งหมด (รวมถึง AUR packages):**
    ```
    $ yay
    # หรือ
    $ yay -Syu
    ```

    นี่คือคำสั่งที่คุณจะใช้บ่อยที่สุด yay จะตรวจสอบทั้ง official repositories และ AUR เพื่ออัปเดตแพ็คเกจทั้งหมด

- **ติดตั้งแพ็คเกจจาก AUR:**
    ```
    $ yay -S <package_name>
    ```
    `yay` จะค้นหาแพ็คเกจใน AUR หากพบจะแสดงข้อมูลและถามยืนยันการติดตั้ง
    ตัวอย่าง: ติดตั้ง Visual Studio Code (ซึ่งมักจะอยู่ใน AUR)
    ```
    $ yay -S visual-studio-code-bin
    ```
    `yay` จะดาวน์โหลด PKGBUILD, ตรวจสอบ dependencies, และรัน `makepkg` ให้โดยอัตโนมัติ

- **ค้นหาแพ็คเกจ (ทั้ง official และ AUR):**
    ```
    $ yay -Ss <keyword>
    ```
    `yay` จะแสดงผลลัพธ์ทั้งจาก official repositories และ AUR

- **ลบแพ็คเกจ (ทั้ง official และ AUR):**
    ```
    $ yay -R <package_name>
    # หรือ
    $ yay -Rs <package_name>
    ```
    ทำงานเหมือน `pacman -R` และ `pacman -Rs`

- **แสดงข้อมูลแพ็คเกจ (ทั้ง official และ AUR):**
    ```
    $ yay -Qi <package_name>  # สำหรับแพ็คเกจที่ติดตั้งแล้ว
    $ yay -Si <package_name>  # สำหรับแพ็คเกจที่ยังไม่ได้ติดตั้ง
    ```

- **ล้าง Package Cache:**
    ```
    $ yay -Sc
    ```
    ทำงานคล้ายกับ `pacman -Sc` แต่ `yay` อาจมีการจัดการ cache สำหรับ AUR packages เพิ่มเติมด้วย