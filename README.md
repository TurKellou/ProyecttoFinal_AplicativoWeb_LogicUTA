# Aplicativo Interactivo de Logica y Algoritmos

**Asignatura:** Algoritmos y Logica de Programacion  
**Universidad:** Universidad Tecnica de Ambato  
**Docente:** Jose Caiza  
**Grupo:** A — Balarezo Diego, Bravo Samuel, Cajiao Paulo, Loor Jhon, Pomaquero Katherine  

---

## Descripcion General

Este repositorio contiene el proyecto final de la asignatura Algoritmos y Logica de Programacion. Se trata de un aplicativo web interactivo orientado a la ensenanza y evaluacion de conceptos fundamentales de programacion, desarrollado con Django como framework principal e integrado con implementaciones en C++ y Python que sirven como base pedagogica del sistema.

El aplicativo permite a los estudiantes interactuar con ejercicios de logica, ingresar respuestas, recibir retroalimentacion automatica y llevar un seguimiento de su progreso academico.

---

## Objetivos

**General**

Desarrollar un sistema de gestion academica integral que permita la administracion de informacion estudiantil y temas curriculares, garantizando la integridad de los datos mediante el uso de estructuras de control y persistencia en archivos externos.

**Especificos**

- Implementar estructuras de datos (arreglos y structs) para organizar eficientemente las notas de los estudiantes y las descripciones tecnicas de los temas de programacion vistos en clase.
- Disenar un menu interactivo que facilite la navegacion del usuario entre las funciones de ingreso, visualizacion y almacenamiento, aplicando logica de control de flujo.
- Gestionar la persistencia de datos mediante flujos de salida (`ofstream` en C++, `open()` en Python), permitiendo el registro historico de actividades y promedios en un archivo de texto plano.

---

## Funcionalidades del Aplicativo Web

- Registro e inicio de sesion de estudiantes.
- Visualizacion de ejercicios clasificados por tema y nivel de dificultad.
- Evaluacion automatica de respuestas numericas (con tolerancia de 0.01) y textuales.
- Visualizacion del codigo educativo en C++ y Python con resaltado de sintaxis.
- Exportacion del historial de intentos en formato CSV.
- Retroalimentacion detallada por ejercicio.
- Seguimiento del progreso por estudiante.

---

## Stack Tecnologico

| Componente | Version | Funcion Principal |
|---|---|---|
| Django | 6.0.4 | Framework principal, arquitectura backend y gestion de rutas |
| Python | 3.x (CPython 3.14) | Lenguaje base del backend y logica de evaluacion |
| SQLite3 | Incluida en Django | Motor de base de datos relacional para entorno de desarrollo |
| HTML5 / CSS3 | Estandar | Estructura semantica y diseno visual de las interfaces |
| JavaScript | ES6+ | Interactividad en el cliente y manipulacion del DOM |
| Highlight.js | 11.9.0 (CDN) | Resaltado de sintaxis de C++ y Python en el navegador |
| Django Template Language | Incluida | Motor de plantillas para renderizado dinamico desde el servidor |
| CSV (modulo Python) | Libreria Estandar | Exportacion del historial de intentos |

---

## Estructura de la Base de Datos

| Modelo | Relacion | Modelo Relacionado | Tipo | Descripcion |
|---|---|---|---|---|
| Ejercicio | Pertenece a | Tema | ForeignKey (N:1) | Varios ejercicios pueden estar categorizados bajo un mismo tema academico |
| Intento | Registrado por | Usuario | ForeignKey (N:1) | Un estudiante puede realizar multiples intentos para resolver un ejercicio |
| Intento | Corresponde a | Ejercicio | ForeignKey (N:1) | Cada intento esta vinculado a un ejercicio especifico del sistema |
| Retroalimentacion | Uno a uno con | Ejercicio | OneToOneField (1:1) | Cada ejercicio tiene una unica guia de solucion o feedback detallado |
| ProgresoEstudiante | Calculado para | Usuario | ForeignKey (N:1) | Almacena el historial de metricas y avance para cada usuario |

---

## Temas Curriculares Abordados

Los ejercicios del aplicativo cubren los siguientes contenidos de la asignatura:

- Variables y tipos de datos
- Condicionales (`if/else`, `switch`)
- Ciclos (`for`, `do-while`, `while`)
- Arreglos unidimensionales y estructuras (`struct`)
- Acumuladores y contadores
- Menus interactivos
- Persistencia en archivos
- Validaciones de entrada
- Organizacion modular del codigo

---

## Implementacion en C++ y Python

El nucleo logico del sistema — un gestor de notas academicas — fue desarrollado en C++ y replicado en Python como ejercicio comparativo. Ambas implementaciones comparten la misma arquitectura de 15 partes documentadas, que incluyen:

1. Definicion de estructuras de datos (`struct Tema` / `class Tema`)
2. Declaracion de variables y arreglos
3. Menu interactivo mediante ciclo `do-while` / `while True`
4. Ingreso de notas con ciclo `for`
5. Calculo de promedio y uso de bandera booleana (`notasIngresadas`)
6. Reporte en pantalla con validacion de estado
7. Visualizacion de temas teoricos mediante recorrido de arreglos
8. Exportacion de datos al archivo `resultados.txt` en modo `append`
9. Validacion de entradas no validas

---

## Instrucciones de Instalacion

### Requisitos previos

- Python 3.10 o superior
- pip
- Git

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

# 4. Aplicar migraciones
python manage.py migrate

# 5. Ejecutar el servidor de desarrollo
python manage.py runserver
```

El aplicativo estara disponible en `http://127.0.0.1:8000/`.

---

## Compilacion del Codigo C++

```bash
g++ -o sistema main.cpp
./sistema
```

---

## Repositorio

[https://github.com/TurKellou/ProyectoProgramacionGrupoA.git](https://github.com/TurKellou/ProyectoProgramacionGrupoA.git)

---

## Conclusiones

El proyecto demuestra la aplicacion practica de los conceptos fundamentales de la asignatura en un entorno de desarrollo real. La implementacion de arreglos y estructuras permitio organizar eficientemente los datos; el diseno del menu interactivo con validaciones garantizo la robustez de la interfaz; y la gestion de la persistencia en archivos aseguro que la informacion academica trascienda la memoria volatil del sistema, sentando las bases para la construccion de aplicaciones escalables.

---

*Proyecto desarrollado en el marco de la asignatura Algoritmos y Logica de Programacion — Universidad Tecnica de Ambato, 2026.*
