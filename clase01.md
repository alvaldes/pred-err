# Clase 01 - 27/08/25

## Definiciones

Un **bug** se refiere a cualquier inconsistencia entre un resultado real y el esperado en el funcionamiento.
Debe ser en el entorno de desarrollo.

### Tipos de Bugs

- Funcionales: afectan la funcionalidad del software.
- Lógico: cuando se comporta de manera incorrecta en una forma no intencionada o no anticipada.
- Comando 0 algorítmico
- Integración
- Seguridad
- Fuera de limite

## Términos

Error: equivocación hecha por un programador durante la codificación

Bug: error detectado en el ambiente de desarrollo durante la etapa de pruebas.

Defecto: incompatibilidad entre el resultado esperado y el real en el desarrollo de software detectado por un desarrollador o cliente final en el ambiente de producción.

Falla: es un error encontrado por el usuario final.

> [!NOTE]
> Entonces: Error --> produce Defecto/Bug --> produce Falla

## Tipos de defectos

### Dependiendo del aspecto que afectan

- Integración: interacción incorrecta entre multiples módulos o componentes del sistema. Se debe aplicar pruebas de integración.
- Rendimiento: defecto que afecta la calidad del rendimiento del producto, como velocidad del procesamiento o uso eficiente de recursos.
- Lógico: un mal funcionamiento que produce resultados incorrectos de procesamiento de solicitud o cualquier otro comportamiento no esperado.
- Funcional
- Usabilidad
- Seguridad
- Compatibilidad

### Por Severidad

- Critico/catastrófico: falla total o perdida irrecuperable de datos.
- Grave: impacta significativamente la funcionalidad y rendimiento del programa.
- Menor: impacto menor en el rendimiento de la app, tal como causar que se ejecute mas lento. Debe documentarse.
- Trivial: no afecta el funcionamiento del producto o la calidad de la experiencia de usuario. Se puede mejorar.

### Por nivel de prioridad de acuerdo a la seriedad del defecto

- Bajo
- Mediano
- Alto
- Urgente
