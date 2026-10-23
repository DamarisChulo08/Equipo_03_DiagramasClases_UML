Caso 1 – Sistema Hospitalario HealthFlow - Diccionario Técnico

Modelo UML Hospital

Justificación de Herencia: Médico, Enfermero y Administrativo comparten atributos comunes (id, nombre, dui, teléfono) La clase padre es abstracta `Empleado` para evitar instanciación directa y promover reutilización.

Medico: Su función es diagnosticar a los pacientes y recetarles medicina, procesos tipo void.

Enfermero: Su función es asistir al paciente, administrar medicina a los pacientes y administrar áreas de trabajo. Sus procesos son tipo void.

Administrativo: Su función es gestionar agenda de citas en el hospital y administrar fechas. Procesos tipo void.

Diagrama 2- Composición Fuerte: Paciente y ExpedienteClínico

Justificación: Es una relación de dependencia existencial y ciclo de vida compartido. Un ExpedienteClínico no tiene sentido ni puede existir en el sistema sin un Paciente asociado. Si se elimina el Paciente, el sistema debe eliminar en cascada su Expediente. Multiplicidad 1 a 1.

Paciente: Persona que recibe la consulta y sobre quien se hace el expediente. Sus atributos son: (Numero de usuario, nombre, apellido, dui, historia). Los métodos que ejecuta es recibir la consulta y recibir el diagnostico.

Expediente: Este contiene la información del paciente, también es lo que se crea para mantener la info del paciente. Procesos tipo void.

Diagrama 3- Asociacion directa: Consulta y receta

Justificación: Esta relación es asociación simple. Debido a que la clase receta se genera de la consulta

Consulta: Aca se define que tiene el paciente. Posee varios datos como la fecha de consulta, el diagnostico y que se le dara al paciente como tratamiento, Su proceso es el de definir diagnostico del paciente.

Receta: Esta contiene la info de la medicinas y aplicación del paciente. Posee proceso de tipo void.

Diagrama 3- Agregación: SalaQuirúrgica — EquipoMédico — Insumos

Agregación (diamante blanco)

Es una relación "todo-parte" débil. Una Sala Quirúrgica tiene Equipo Médico, pero el equipo puede ser movido a otra sala, estar en mantenimiento o en bodega. Sigue existiendo sin la sala.

Sus procesos son

Insumos: Estos tienen la función de aplicarse en los pacientes que lo necesiten, se compone de fecha de vencimiento, id, nombre y cantidad

Equipo Medico: Estos sirven como apoyo para aplicar los insumos en los pacientes. Proceso de tipo void

Sal quirúrgica. Esta clase es donde se realiza la aplicación de los métodos anteriores. Se compone de id, nombre, capacidad y estado. Procesos de tipo void
