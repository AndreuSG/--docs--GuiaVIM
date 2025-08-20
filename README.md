# Guía de VIM en Español

El objetivo de este documento es aprender Vim desde cero hasta un nivel intermedio para poder editar ficheros de forma rápida y segura en cualquier servidor.  
Este documento está pensado para **profesionales de TI** que necesitan **manejar Vim en servidores remotos** sin depender de configuraciones avanzadas.

## 1. Introducción a Vim

### ¿Qué es Vim?
- **Vim** significa **Vi IMproved** (una versión mejorada de `vi`).
- Es un editor de texto en modo consola, muy ligero y rápido.
- Viene preinstalado en la mayoría de sistemas UNIX y Linux.
- Para un SysAdmin/DevOps es esencial porque:
  - Siempre estará disponible en servidores (a diferencia de `nano`).
  - Permite editar archivos de configuración de forma eficiente.
  - Funciona incluso en entornos muy limitados (rescue mode, contenedores).

###
- `vi` → El editor original (muy básico).
- `vim` → Versión mejorada de `vi`, con más comandos, sintaxis coloreada, etc.
- `nvim` (Neovim) → Versión moderna con más funcionalidades, pero **no siempre estará en servidores**.

> **Regla básica**: Domina `vim` primero, luego usa Neovim en tu máquina local para productividad.

---

### Modos de Vim
Vim no funciona como los editores de texto normales (VSCode, nano, Sublime…).  
Su poder está en que **tiene distintos modos de trabajo**:

1. **Normal mode (modo normal)**  
   - Es el modo por defecto al abrir Vim.  
   - Sirve para **moverte, copiar, pegar, borrar, buscar**.  
   - Aquí no puedes escribir texto directamente.  

2. **Insert mode (modo inserción)**  
   - Aquí sí puedes escribir texto como en cualquier editor.  
   - Se activa con `i` (insertar antes del cursor) o `a` (insertar después).  
   - Se vuelve al modo normal pulsando `<Esc>`.

3. **Visual mode (modo visual)**  
   - Para seleccionar bloques de texto.  
   - `v` = selección carácter por carácter.  
   - `V` = selección por líneas.  
   - `<C-v>` = selección por columnas/bloques.

4. **Command-line mode (modo comando)**  
   - Se activa con `:` en modo normal.  
   - Permite ejecutar órdenes como `:w` (guardar), `:q` (salir), `:wq` (guardar y salir).  
   - También ejecutar comandos del sistema con `:!`.

---

# Comandos básicos de Vim

Esta guía contiene los **comandos esenciales de Vim** para empezar a usarlo en cualquier servidor.  
Todos los comandos se ejecutan desde el **modo normal** (pulsa `<Esc>` para asegurarte de estar en ese modo).

---

## 1. Abrir y salir
- `vim archivo.txt` → abrir un archivo.
- `:w` → guardar.
- `:q` → salir (si no hay cambios).
- `:wq` → guardar y salir.
- `:x` → guardar y salir (equivalente a `:wq`).
- `:q!` → salir sin guardar.
- `ZZ` → guardar y salir rápidamente (sin `:`).

---

## 2. Insertar texto (entrar en modo insert)
- `i` → insertar **antes** del cursor.
- `a` → insertar **después** del cursor.
- `o` → abre una línea **debajo** y entra en insert.
- `O` → abre una línea **encima** y entra en insert.
- `<Esc>` → volver a modo normal.

---

## 3. Movimiento dentro del texto

- `h` → izquierda.
- `l` → derecha.
- `j` → abajo.
- `k` → arriba.
- También puedes usar las flechas del teclado, pero es mejor aprender los comandos de movimiento por si hay que trabajar en un entorno sin soporte para flechas como en un vi normal.
- `:{n}` → ir a la línea número *n*. (ej: `:25` va a la línea 25).
- `0` → inicio de la línea.
- `$` → final de la línea.
- `w` → siguiente palabra.
- `e` → final de la palabra.
- `b` → inicio de la palabra anterior.
- `gg` → ir al inicio del archivo.
- `G` → ir al final del archivo.

---

## 4. Copiar, pegar y cortar
- `yy` → copiar (yank) una línea.
- `nyy` → copiar *n* líneas (ej: `3yy` copia 3 líneas).
- `p` → pegar **debajo** de la línea actual.
- `P` → pegar **encima** de la línea actual.
- `dd` → cortar (delete) una línea.
- `ndd` → cortar *n* líneas (ej: `2dd` corta 2 líneas).
- `D` → borrar desde el cursor hasta final de línea.
- `x` → borrar un carácter.

---

## 5. Deshacer y rehacer
- `u` → deshacer (undo).
- `Ctrl+r` → rehacer (redo). **Nota**: Debes mantener presionado `Ctrl` y luego pulsar `r`.
- **Importante sobre redo**: El redo solo funciona si NO has hecho cambios nuevos después del undo. Si haces undo y luego escribes algo nuevo, pierdes la posibilidad de hacer redo.

### Ejemplo de uso de undo/redo:
1. Escribes texto → `u` (undo) → se borra
2. `Ctrl+r` (redo) → vuelve el texto
3. Pero si haces: escribes → `u` → escribes algo nuevo → `Ctrl+r` **NO funcionará**

---

## 6. Búsqueda y reemplazo
- `/texto` → buscar hacia adelante.
- `?texto` → buscar hacia atrás.
- `n` → repetir búsqueda en la misma dirección.
- `N` → repetir búsqueda en la dirección opuesta.
- `:%s/viejo/nuevo/g` → reemplazar todas las ocurrencias en el archivo.
- `:%s/viejo/nuevo/gc` → reemplazar con confirmación.

---

## 7. Selección de texto (modo visual)
- `v` → modo visual (carácter por carácter).
- `V` → modo visual por líneas.
- `<C-v>` → modo visual en bloque/columna.
- `y` → copiar selección.
- `d` → cortar selección.
- `p` → pegar después de la selección.

---

## 8. 📄 Manejo de múltiples archivos
- `:e archivo.txt` → abrir otro archivo en el mismo Vim.
- `:ls` → listar buffers abiertos.
- `:bN` → ir al buffer número N.
- `:bn` → siguiente buffer.
- `:bp` → buffer anterior.

---

## 9. 🪄 Comandos útiles extra
- `.` → repetir el último comando.
- `:%d` → borrar todo el contenido del archivo.
- `:!comando` → ejecutar un comando de la shell (ej: `:!ls`).
- `:%!comando` → aplicar un comando al contenido (ej: `:%!sort` ordena todas las líneas).

---

## 10. 🎨 Personalización y configuración
- Vim se puede personalizar con un archivo `.vimrc` en tu directorio home.
- Para crear/editar: `vim ~/.vimrc`

### Configuración básica:
```vim
" ===== CONFIGURACIÓN BÁSICA =====
set number              " Mostrar números de línea
set relativenumber      " Números de línea relativos
set tabstop=4           " Ancho de tabulación de 4 espacios
set shiftwidth=4        " Indentación de 4 espacios
set expandtab           " Convertir tabulaciones a espacios
set autoindent          " Mantener indentación al crear nuevas líneas
set smartindent         " Indentación inteligente
set wrap                " Ajustar líneas largas
set linebreak           " No cortar palabras al ajustar líneas
```

### Búsqueda y navegación:
```vim
" ===== BÚSQUEDA Y NAVEGACIÓN =====
set hlsearch            " Resaltar resultados de búsqueda
set incsearch           " Búsqueda incremental
set ignorecase          " Ignorar mayúsculas/minúsculas en búsquedas
set smartcase           " Sensible a mayúsculas si incluyes mayúsculas
set showcmd             " Mostrar comandos parciales
set wildmenu            " Menú de autocompletado mejorado
set wildmode=longest,list,full " Modo de autocompletado
```

### Apariencia y colores:
```vim
" ===== APARIENCIA Y COLORES =====
syntax on               " Activar resaltado de sintaxis
set background=dark     " Fondo oscuro (o 'light' para claro)
colorscheme desert      " Tema de colores (otros: blue, evening, koehler, murphy, pablo, peachpuff, ron, shine, slate, torte, zellner)
set cursorline          " Resaltar línea actual
set ruler               " Mostrar posición del cursor
set laststatus=2        " Siempre mostrar barra de estado
set showmode            " Mostrar modo actual
set title               " Mostrar título en la ventana del terminal
```

### Temas de colores incluidos en Vim:
```vim
" Para cambiar tema, usa uno de estos:
" colorscheme blue
" colorscheme darkblue
" colorscheme default
" colorscheme delek
" colorscheme desert
" colorscheme elflord
" colorscheme evening
" colorscheme industry
" colorscheme koehler
" colorscheme morning
" colorscheme murphy
" colorscheme pablo
" colorscheme peachpuff
" colorscheme ron
" colorscheme shine
" colorscheme slate
" colorscheme torte
" colorscheme zellner
```

### Funcionalidades útiles:
```vim
" ===== FUNCIONALIDADES ÚTILES =====
set clipboard=unnamedplus " Usar portapapeles del sistema (Linux)
" set clipboard=unnamed   " Para macOS
set mouse=a             " Habilitar ratón en todos los modos
set scrolloff=8         " Mantener 8 líneas visibles arriba/abajo del cursor
set sidescrolloff=8     " Mantener 8 columnas visibles a los lados
set history=1000        " Guardar más historial de comandos
set undolevels=1000     " Más niveles de deshacer
```

### Configuración avanzada:
```vim
" ===== CONFIGURACIÓN AVANZADA =====
set noswapfile          " No crear archivos .swp
set nobackup            " No crear copias de seguridad
set encoding=utf-8      " Codificación UTF-8
set fileencoding=utf-8  " Codificación de archivos
set backspace=indent,eol,start " Comportamiento del backspace
set splitright          " Nuevas ventanas verticales a la derecha
set splitbelow          " Nuevas ventanas horizontales abajo
```

### Para aplicar cambios:
1. Guarda el archivo `.vimrc`
2. Reinicia Vim o ejecuta `:source ~/.vimrc`

### Comandos para cambiar tema en tiempo real:
- `:colorscheme desert` → cambiar a tema desert
- `:colorscheme` → ver tema actual
- `Tab` después de `:colorscheme ` → autocompletar temas disponibles