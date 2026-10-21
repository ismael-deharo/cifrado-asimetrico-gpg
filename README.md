# Implementación de Cifrado Asimétrico con GPG para Transmisión de Secretos

**Entorno Técnico:**
- Ubuntu 24.04 LTS
- GnuPG (GPG)
- Criptografía RSA (4096 bits)

**Objetivo Empresarial:**
Garantizar la confidencialidad e integridad en el intercambio de credenciales de infraestructura entre administradores de sistemas mediante cifrado de clave pública.

**Implementación y Comandos:**

1. **Generación de pares de claves criptográficas**
   ```bash
   gpg --full-generate-key
   
Verificación: Ejecutar gpg --list-keys para confirmar la creación de los anillos de claves con el algoritmo RSA de 4096 bits.

2. **Extracción de la clave pública**
   ```bash
   gpg -a --export admin-a@empresa.local > adminA.pub.asc
   gpg -a --export admin-b@empresa.local > adminB.pub.asc

Verificación: Lanzar cat adminA.pub.asc. El sistema debe devolver un bloque de texto que comienza por -----BEGIN PGP PUBLIC KEY BLOCK-----.

3. **Importación de claves externas en el anillo de confianza**
   ```bash
   gpg --import adminB.pub.asc

Verificación: Usar gpg --list-keys. La firma digital del administrador externo figurará en el listado de claves autorizadas.

4. **Cifrado de la carga útil**
   ```bash
   echo "DB_PASS=Clave$1" > secreto.txt
   gpg -a -r admin-b@empresa.local --encrypt secreto.txt

Verificación: Revisar el directorio con ls -la. Existe un nuevo archivo llamado secreto.txt.asc. Su lectura muestra un bloque PGP ilegible.

5. **Descifrado mediante clave privada local**
   ```bash
   gpg -a --decrypt secreto.txt.asc > secreto_descifrado.txt
