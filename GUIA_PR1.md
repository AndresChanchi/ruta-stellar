---
title: "Guía paso a paso · PR #1: tu ficha de participante"
subtitle: "Ruta JaveBlockchain 2026-II · Semana 1 · Entrega: lunes 20 de octubre, 11:59 p. m."
author: "Semillero JaveBlockchain · Pontificia Universidad Javeriana"
geometry: "margin=2cm"
fontsize: 11pt
colorlinks: true
---

# Qué vas a entregar

Esta semana la tarea es **individual**: los equipos se arman el viernes 23 con la información que pongas aquí. Vas a crear **un archivo con tu ficha** en el repositorio central del semillero y entregarlo como **pull request (PR)**.

| | Todos | Builders, además |
|---|---|---|
| 1 | Archivo `participantes/<tu-usuario>.md` con tu nombre, nivel y dirección G… | Rust y `stellar-cli` instalados |
| 2 | Explicación propia (máx. 150 palabras) de tu primera transacción y su hash | Llave `alice` creada y fondeada en testnet |
| 3 | La sección «Para armar equipos» diligenciada | Salida de `stellar --version` y `stellar keys address alice` pegada en tu ficha |
| 4 | Declaración de uso de IA en la plantilla del PR | `cargo test` en verde en `template-builders` |

Tiempo estimado: 40 minutos para Fundamentos; 1,5 a 2 horas para Builders (instalar Rust es lo que más tarda). No lo dejes para el lunes en la noche: el martes hay office hours para lo que no funcione.

**Por qué es fácil a propósito:** lo que medimos esta semana no es código, sino que el flujo *fork → rama → commit → PR → revisión* le funcione a cada persona antes de que haya contratos de por medio. Y tu ficha es lo que vamos a usar para armar equipos equilibrados.

# Parte 0 · Lo que necesitas tener

- Cuenta en [github.com](https://github.com). Usa un nombre de usuario que quieras mostrar en tu hoja de vida.
- Git instalado. Verifica en una terminal: `git --version`. Si no está:
  - **Windows:** [Git for Windows](https://git-scm.com/download/win); usa *Git Bash* como terminal.
  - **macOS:** `xcode-select --install` o `brew install git`.
  - **Linux:** `sudo apt install git`.
- Configura tu nombre una sola vez:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu-correo@javeriana.edu.co"
```

- Un editor: VS Code o Cursor.
- Tu dirección pública de testnet (empieza por `G`, 56 caracteres): en Freighter, clic en tu cuenta → *Copy address*.
- El hash de tu primera transacción (64 caracteres). Si no lo guardaste: abre [stellar.expert/explorer/testnet](https://stellar.expert/explorer/testnet), pega tu dirección G… y busca en *Payments* el pago de 10 XLM del viernes.

¿Prefieres no usar la terminal? [GitHub Desktop](https://desktop.github.com) hace fork, clone, rama, commit y push con botones. Los pasos son los mismos; solo cambia cómo los ejecutas.

# Parte 1 · Fork y clone del repositorio central

El repositorio es público: `https://github.com/javeblockchain/ruta-2026-II`. Tú no escribes directo en él; haces una copia a tu cuenta (*fork*), trabajas ahí y luego propones tus cambios (*pull request*). Es exactamente como se contribuye a cualquier proyecto de código abierto.

1. Abre el repositorio en GitHub y haz clic en **Fork** (arriba a la derecha) → **Create fork**. Ahora existe `github.com/<tu-usuario>/ruta-2026-II`.
2. En **tu** fork, botón verde **Code** → **HTTPS** → copia la URL.
3. En la terminal:

```bash
cd ~/Documents                       # o la carpeta donde guardes proyectos
git clone https://github.com/<tu-usuario>/ruta-2026-II.git
cd ruta-2026-II
ls participantes/                    # debes ver PLANTILLA.md
```

Si al hacer push GitHub pide contraseña y la rechaza: no es la de tu cuenta, necesitas un *Personal Access Token* (GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token, permiso `repo`). Pégalo donde pide la contraseña.

# Parte 2 · Crear una rama

```bash
git checkout -b ficha/<tu-usuario>
```

`git branch` muestra en qué rama estás (la del `*`).

# Parte 3 · Crear tu ficha

1. Copia la plantilla con tu usuario de GitHub como nombre de archivo (minúsculas, sin espacios):

```bash
cp participantes/PLANTILLA.md participantes/<tu-usuario>.md
```

2. Abre `participantes/<tu-usuario>.md` en tu editor y reemplaza **todo** lo que esté entre `< >`:
   - **Tabla de datos:** usuario, carrera y semestre, nivel (Fundamentos o Builders; si dudas, Fundamentos: se puede subir en cualquier sesión), dirección `G…` copiada desde Freighter, hash y enlace al explorador.
   - **«Mi primera transacción»:** máximo 150 palabras, escritas por ti. Debe responder: ¿qué construyó Freighter cuando presionaste *Send*? ¿Con qué llave se firmó y por qué esa llave nunca salió de tu navegador? ¿Quién pagó el fee y cuánto fue? ¿Cómo comprobaste que ocurrió? No hay una única respuesta correcta; sí hay una incorrecta: copiar de internet. En la sesión 4 te lo preguntamos en voz alta.
   - **«Para armar equipos»:** sé concreto. «Sé React y un poco de Python», «quiero aprender a escribir contratos», «me gustaría resolver las filas de la cafetería». Con esto armamos equipos de 3 a 4 personas mezclando niveles el viernes.
   - **Declaración de IA:** una línea honesta.
3. **No edites `PLANTILLA.md`** ni archivos de otras personas.

Revisa qué cambió:

```bash
git status      # participantes/<tu-usuario>.md aparece como nuevo
```

# Parte 4 · Solo Builders: entorno de desarrollo

Si eres de Fundamentos, salta a la Parte 5.

## 4.1 Instalar Rust

- **macOS y Linux:**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
  Opción 1 (por defecto). Cierra y vuelve a abrir la terminal.

- **Windows:** WSL2 con Ubuntu (recomendado): en PowerShell como administrador `wsl --install`, reinicia, abre Ubuntu y corre el comando de arriba. Si prefieres Windows nativo: `rustup-init.exe` desde [rustup.rs](https://rustup.rs); te pedirá instalar antes las *Visual Studio C++ Build Tools*.

Verifica: `rustc --version` (1.84.0 o superior) y `cargo --version`.

## 4.2 Target de WebAssembly

```bash
rustup target add wasm32v1-none
```

## 4.3 Instalar stellar-cli

- **macOS (Homebrew):** `brew install stellar-cli`
- **Linux, macOS sin Homebrew y WSL:**

```bash
curl -fsSL https://github.com/stellar/stellar-cli/raw/main/install.sh | sh
```

- **Si lo anterior falla** (tarda 10–20 min porque compila): `cargo install --locked stellar-cli`

Verifica: `stellar --version`. Guía oficial por si algo cambió: [developers.stellar.org → Getting Started → Setup](https://developers.stellar.org/docs/build/smart-contracts/getting-started/setup).

## 4.4 Crear y fondear tu llave de pruebas

```bash
stellar keys generate alice --network testnet --fund
stellar keys address alice
```

El segundo comando imprime una `G…`: es la cuenta `alice`, con 10 000 XLM de testnet. Vive en tu computador, no en Freighter; son cuentas distintas y está bien.

## 4.5 Comprobar que puedes compilar un contrato

Clona la plantilla de Builders (en otra carpeta, fuera de `ruta-2026-II`) y corre las pruebas:

```bash
cd ~/Documents
git clone https://github.com/javeblockchain/template-builders.git
cd template-builders
cargo test
```

La primera vez descarga y compila el SDK (2 a 5 minutos). Debe terminar en `test result: ok. 2 passed`. Si lo ves, tu entorno está listo para el viernes. **No corras `cargo update`**: el `Cargo.lock` fija versiones que sabemos que compilan.

## 4.6 Pegar la evidencia en tu ficha

En la sección «Solo Builders: entorno» de tu archivo pega como **texto** la salida de `rustc --version`, `stellar --version` y `stellar keys address alice`. Añade una línea: `cargo test: 2 passed`.

# Parte 5 · Commit

```bash
git add participantes/<tu-usuario>.md
git commit -m "docs: ficha de <tu nombre>"
```

Un commit por cambio, mensaje `tipo: descripción` (ver `CONTRIBUTING.md`). `git log --oneline` muestra tus commits. Si el mensaje salió mal y aún no hiciste push: `git commit --amend -m "mensaje nuevo"`.

# Parte 6 · Push y pull request

1. Sube la rama a **tu fork**:

```bash
git push -u origin ficha/<tu-usuario>
```

2. Abre tu fork en GitHub. Aparece un aviso *«ficha/<tu-usuario> had recent pushes»* con el botón **Compare & pull request**. Haz clic. Verifica arriba que diga: base repository `javeblockchain/ruta-2026-II`, base `main` ← head repository `<tu-usuario>/ruta-2026-II`, compare `ficha/<tu-usuario>`.
3. Título: `PR #1 · <tu nombre>`.
4. La descripción viene **prellenada con la plantilla**. No la borres: llénala.
   - *Qué hace este PR:* «Agrega mi ficha de participante».
   - *Cómo lo probamos:* marca lo que aplique (para este PR basta con que el enlace al explorador abra tu transacción).
   - *Declaración de uso de IA:* marca lo verdadero. Si usaste IA para redactar el párrafo, dilo y di qué cambiaste. Usarla no resta puntos; ocultarla sí.
   - *Quién puede explicar cada parte:* `participantes/<tu-usuario>.md → yo`.
5. **Create pull request**.
6. Revisión: el mentor revisa cada PR el martes. Además, **comenta el PR de otra persona** del grupo (el que esté justo encima del tuyo en la lista de *Pull requests*): algo sustantivo, no «LGTM». Ejemplo: «en el párrafo dices que el banco pagó el fee, pero fue tu cuenta origen». Revisar también es parte de la tarea.
7. Cuando el mentor apruebe, él lo fusiona. Verás tu archivo en `participantes/` del repo central.

Si después de abrir el PR te piden cambios: edita el archivo, `git add`, `git commit`, `git push`. El PR se actualiza solo; no abras otro.

# Parte 7 · Checklist antes de dar por entregado

- [ ] El PR está abierto contra `javeblockchain/ruta-2026-II` antes del lunes 20 a las 11:59 p. m.
- [ ] El archivo se llama exactamente `participantes/<tu-usuario>.md` y no tocaste `PLANTILLA.md`.
- [ ] La dirección `G…` tiene 56 caracteres y el enlace al explorador abre tu transacción.
- [ ] El párrafo tiene 150 palabras o menos y lo escribiste tú.
- [ ] «Para armar equipos» está completo.
- [ ] La declaración de IA está diligenciada en el PR.
- [ ] Comentaste el PR de otra persona.
- [ ] Builders: `cargo test` dice `2 passed` y la evidencia está como texto.
- [ ] No subiste ninguna llave secreta (`S…`) ni frase de 12 palabras. Comprueba: `git grep -n "S[A-Z2-7]\{55\}"` no debe imprimir nada.

# Problemas frecuentes

| Síntoma | Causa probable | Solución |
|---|---|---|
| `git push` pide contraseña y la rechaza | GitHub no acepta contraseñas por HTTPS | Personal Access Token (Parte 1) o GitHub Desktop |
| `Permission denied` al hacer push | Clonaste el repo central y no tu fork | Haz fork, clona **tu** copia y vuelve a intentar |
| El PR no muestra la plantilla | Abriste el PR dentro de tu propio fork | Verifica que la base sea `javeblockchain/ruta-2026-II` |
| Conflicto en el PR | Alguien editó el mismo archivo | No debería pasar: cada persona tiene su archivo. Si pasa, avísame |
| `rustc: command not found` tras instalar | La terminal no recargó el PATH | Cierra y abre la terminal; en Linux/macOS `source ~/.cargo/env` |
| `error: linker 'cc' not found` (Linux/WSL) | Faltan herramientas de compilación | `sudo apt install build-essential` |
| `stellar: command not found` | Binario fuera del PATH | Reabre la terminal; si persiste, `cargo install --locked stellar-cli` |
| `cargo test` falla con errores de `ed25519-dalek` o `rand` | Corriste `cargo update` o borraste `Cargo.lock` | `git checkout Cargo.lock` y vuelve a correr |
| Friendbot no fondea a `alice` | Red caída o cuota | Reintenta `stellar keys fund alice --network testnet` o pide XLM en el grupo |
| Mi hash no aparece en stellar.expert | Buscas en mainnet | La URL debe decir `/explorer/testnet` |

Si nada de esto resuelve tu caso, tráelo a las office hours del martes con el mensaje de error copiado (texto, no foto).

# Qué pasa después

El viernes 23, con las fichas en mano, armamos los equipos en los primeros 15 minutos de la sesión y cada equipo recibe su repositorio creado desde la plantilla de su nivel. Luego Fundamentos emite su propio activo en Stellar Lab y Builders compila, despliega e invoca `hello_world` en testnet. Lectura para llegar preparados: [Stellar Docs → Learn → Fundamentals](https://developers.stellar.org/docs/learn/fundamentals) (Accounts, Assets, Operations, Transactions).
