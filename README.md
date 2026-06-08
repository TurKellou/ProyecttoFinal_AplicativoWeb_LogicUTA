# LogicWeb UTA — Python Edition

**Asignatura:** Algoritmos y Logica de Programacion  
**Universidad:** Universidad Tecnica de Ambato  
**Grupo:** A — Balarezo Diego, Bravo Samuel, Cajiao Paulo, Loor Jhon, Pomaquero Katherine  
**Repositorio:** [https://github.com/TurKellou/ProyectoProgramacionGrupoA.git](https://github.com/TurKellou/ProyectoProgramacionGrupoA.git)

---

## Descripcion General

LogicWeb UTA es una plataforma web didactica para el aprendizaje de Algoritmos y Logica de Programacion. Permite a los estudiantes revisar teoria organizada por unidades, analizar ejercicios resueltos paso a paso e interactuar con un laboratorio de practicas que evalua el razonamiento algoritmico mediante retroalimentacion automatica e inmediata.

Esta version esta desarrollada en Python con el framework Flask, SQLite como motor de base de datos y Jinja2 como sistema de plantillas. Es funcionalmente equivalente a la version original en PHP y MySQL, adaptada a un entorno Python puro sin dependencias externas mas alla de Flask.

---

## Funcionalidades

**Modulo de Teoria — Unidades**

Presenta los temas de la asignatura agrupados por unidad. Cada tema incluye una vista de detalle con contenido HTML enriquecido (definiciones, ejemplos en pseudocodigo y bloques de codigo). Los temas se consultan directamente desde la base de datos y se renderizan via Jinja2.

**Modulo de Ejercicios Resueltos**

Lista los ejercicios clasificados por categoria y nivel de dificultad (Basica, Intermedia, Avanzada). La vista de detalle muestra el enunciado, el analisis IPO (Entrada / Proceso / Salida) y la solucion propuesta con bloques de codigo resaltados.

**Modulo de Practicas — Laboratorio Interactivo**

Cada ejercicio disponible ofrece dos modalidades de practica independientes:

- **Practica con Datos:** El estudiante ingresa valores numericos al formulario, formula una respuesta segun su propio algoritmo y el servidor la evalua via AJAX. La retroalimentacion incluye el valor real calculado, la respuesta del estudiante y una recomendacion pedagogica. La evaluacion se realiza con tolerancia de 0.01 para respuestas numericas y comparacion sin distincion de mayusculas para respuestas textuales.

- **Desafio de Pseudocodigo:** El estudiante recibe las lineas del algoritmo en orden aleatorio (generado con `random.shuffle`) y debe reordenarlas mediante arrastre (drag & drop con SortableJS) hasta reconstruir la secuencia logica correcta. La validacion se realiza via AJAX comparando la cadena de lineas ordenadas separadas por `|` contra el orden correcto almacenado en la aplicacion.

En ambas modalidades, la retroalimentacion se muestra mediante alertas visuales con SweetAlert2. Los intentos se registran en la tabla `Intento` de la base de datos unicamente si el usuario tiene sesion activa.

**Modulo de Usuarios**

- Registro de cuenta con nombre, correo electronico y contrasena. Las contrasenas se almacenan con hash generado por `werkzeug.security.generate_password_hash`.
- Inicio de sesion autenticado verificado con `werkzeug.security.check_password_hash`. La sesion se gestiona con `flask.session`.
- Cierre de sesion mediante `session.clear()`.
- Historial de intentos accesible para usuarios autenticados, con listado ordenado por fecha descendente que incluye ejercicio, categoria, respuesta enviada y resultado.

---

## Stack Tecnologico

| Componente | Version | Funcion |
|---|---|---|
| Python | 3.10+ | Lenguaje base del backend |
| Flask | 3.x | Framework web: rutas, sesiones, plantillas y servidor de desarrollo |
| SQLite3 | Libreria estandar | Motor de base de datos relacional embebido |
| Werkzeug | Incluida en Flask | Hash y verificacion de contrasenas (`pbkdf2:sha256`) |
| Jinja2 | Incluida en Flask | Motor de plantillas para renderizado dinamico del HTML |
| Bootstrap | 5.3.0 (CDN) | Framework CSS para el diseno responsivo de la interfaz |
| Bootstrap Icons | 1.11.1 (CDN) | Iconografia de la interfaz |
| SweetAlert2 | 11 (CDN) | Alertas visuales para retroalimentacion de ejercicios |
| SortableJS | 1.15.0 (CDN) | Drag & drop interactivo en el desafio de pseudocodigo |
| JavaScript | ES6+ (nativo) | Comunicacion AJAX con el servidor via Fetch API |

---

## Estructura del Proyecto

```
logicweb/
├── app.py                          # Aplicacion principal: configuracion, DB y todas las rutas
├── requirements.txt                # Dependencias Python del proyecto
├── instance/
│   └── logicweb.db                 # Base de datos SQLite (generada automaticamente)
└── templates/
    ├── base.html                   # Plantilla base: navbar, estilos globales, footer
    ├── index.html                  # Pagina principal / landing
    ├── login.html                  # Formulario de inicio de sesion
    ├── registro.html               # Formulario de creacion de cuenta
    ├── historial.html              # Historial de intentos del usuario autenticado
    ├── teoria_index.html           # Listado de unidades tematicas
    ├── teoria_detalle.html         # Vista completa de un tema
    ├── resueltos_index.html        # Listado de ejercicios resueltos
    ├── resueltos_detalle.html      # Solucion paso a paso de un ejercicio
    ├── ejercicios_index.html       # Laboratorio: listado de practicas disponibles
    ├── practica.html               # Interfaz de practica con datos y evaluacion AJAX
    └── pseudocodigo.html           # Interfaz de ordenamiento de pseudocodigo drag & drop
```

---

## Rutas de la Aplicacion

| Metodo | Ruta | Funcion | Descripcion |
|---|---|---|---|
| GET | `/` | `index` | Pagina principal |
| GET / POST | `/login` | `login` | Autenticacion de usuarios |
| GET / POST | `/registro` | `registro` | Creacion de cuenta nueva |
| GET | `/logout` | `logout` | Cierre de sesion |
| GET | `/historial` | `historial` | Historial de intentos (requiere sesion) |
| GET | `/teoria` | `teoria_index` | Listado de temas por unidad |
| GET | `/teoria/<id>` | `teoria_detalle` | Detalle de un tema especifico |
| GET | `/resueltos` | `resueltos_index` | Listado de ejercicios resueltos |
| GET | `/resueltos/<id>` | `resueltos_detalle` | Solucion de un ejercicio |
| GET | `/ejercicios` | `ejercicios_index` | Laboratorio de practicas |
| GET | `/ejercicios/practica/<id>` | `practica` | Formulario de practica con datos |
| POST | `/ejercicios/procesar` | `procesar` | Evaluacion serverside (devuelve JSON) |
| GET | `/ejercicios/pseudocodigo/<id>` | `pseudocodigo` | Interfaz de pseudocodigo con lineas aleatorias |
| POST | `/ejercicios/procesar_pseudocodigo` | `procesar_pseudocodigo` | Validacion de orden (devuelve JSON) |

---

## Base de Datos

La base de datos SQLite se crea e inicializa automaticamente al arrancar la aplicacion mediante la funcion `init_db()`. Si las tablas no existen, se crean; si la tabla `Tema` esta vacia, se insertan los datos iniciales.

**Esquema**

```sql
CREATE TABLE Usuario (
    IdUsuario   INTEGER PRIMARY KEY AUTOINCREMENT,
    Nombre      TEXT NOT NULL,
    Correo      TEXT NOT NULL UNIQUE,
    Contrasena  TEXT NOT NULL,              -- Hash pbkdf2:sha256 via Werkzeug
    Rol         TEXT DEFAULT 'Estudiante'
);

CREATE TABLE Tema (
    IdTema      INTEGER PRIMARY KEY AUTOINCREMENT,
    Unidad      TEXT NOT NULL,
    NombreTema  TEXT NOT NULL,
    Descripcion TEXT NOT NULL               -- Contenido HTML enriquecido
);

CREATE TABLE Ejercicio (
    IdEjercicio        INTEGER PRIMARY KEY AUTOINCREMENT,
    IdTema             INTEGER NOT NULL REFERENCES Tema(IdTema),
    Titulo             TEXT NOT NULL,
    Enunciado          TEXT NOT NULL,
    Categoria          TEXT NOT NULL,
    Dificultad         TEXT NOT NULL,
    SolucionEsperada   TEXT,               -- HTML con solucion paso a paso
    LineasPseudocodigo TEXT                -- Lineas separadas por '|'
);

CREATE TABLE Intento (
    IdIntento        INTEGER PRIMARY KEY AUTOINCREMENT,
    IdUsuario        INTEGER NOT NULL REFERENCES Usuario(IdUsuario),
    IdEjercicio      INTEGER NOT NULL REFERENCES Ejercicio(IdEjercicio),
    RespuestaUsuario TEXT,
    Resultado        INTEGER NOT NULL,     -- 1 = correcto, 0 = incorrecto
    FechaIntento     TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Datos iniciales cargados automaticamente**

| Tabla | Registros |
|---|---|
| Tema | 4 (Unidades 1 a 4: Variables, Condicionales, Ciclos, Arreglos) |
| Ejercicio | 2 (Calculo de Promedio, Calculo de Sueldo con Horas Extras) |

---

## Logica de Evaluacion

**Ejercicio 1 — Calculo de Promedio y Estado**

El servidor recibe tres notas numericas, calcula `(n1 + n2 + n3) / 3` y determina el estado: `Aprobado` si el promedio es mayor o igual a 7, `Reprobado` en caso contrario. La comparacion con la respuesta del usuario ignora diferencias de mayusculas (`str.lower()`).

**Ejercicio 2 — Calculo de Sueldo con Horas Extras**

El servidor recibe las horas trabajadas y la tarifa por hora. Si las horas son menores o iguales a 40, el sueldo es `horas * tarifa`. Si superan 40, el sueldo es `(40 * tarifa) + (horas_extra * tarifa * 2)`, donde `horas_extra = horas - 40`. La comparacion es numerica con tolerancia de 0.01.

**Desafio de Pseudocodigo**

El servidor compara la cadena de lineas ordenadas por el usuario (separadas por `|`) contra el orden correcto definido en el diccionario `ordenes_correctos` dentro de `procesar_pseudocodigo`. La comparacion es exacta mediante igualdad de cadenas (`==`).

---

## Instalacion y Ejecucion

### Requisitos

- Python 3.10 o superior
- pip

### Pasos

```bash
# 1. Clonar el repositorio
git clone https://github.com/TurKellou/ProyectoProgramacionGrupoA.git
cd ProyectoProgramacionGrupoA

# 2. Crear y activar el entorno virtual
python -m venv venv
source venv/bin/activate        # Linux / macOS
venv\Scripts\activate           # Windows

# 3. Instalar dependencias
pip install -r requirements.txt

# 4. Ejecutar el servidor de desarrollo
python app.py
```

El aplicativo estara disponible en `http://127.0.0.1:5000`. La base de datos se crea y puebla automaticamente en `instance/logicweb.db` al primer inicio; no se requiere ninguna configuracion adicional.

---

## Diferencias respecto a la Version Original en PHP

| Aspecto | Version PHP | Version Python |
|---|---|---|
| Lenguaje backend | PHP 8.x | Python 3.10+ con Flask 3.x |
| Base de datos | MySQL via PDO | SQLite3 via libreria estandar |
| Plantillas | PHP mezclado con HTML | Jinja2 separado del codigo |
| Sesiones | `$_SESSION` nativo de PHP | `flask.session` con cookie firmada |
| Hash de contrasenas | `password_hash` / `password_verify` (bcrypt) | `werkzeug.security` (pbkdf2:sha256) |
| Estructura de archivos | Un archivo `.php` por vista | Un `app.py` central + carpeta `templates/` |
| Inicializacion de DB | Script SQL externo o phpMyAdmin | `init_db()` automatico al arrancar |
| Servidor de desarrollo | Apache / XAMPP / Laragon | `flask run` o `python app.py` |

---

*Proyecto Formativo Integrador — Universidad Tecnica de Ambato, 2026.*
