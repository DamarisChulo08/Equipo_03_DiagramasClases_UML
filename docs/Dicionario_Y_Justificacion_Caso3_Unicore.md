## 6. Relaciones y decisiones de modelado

* **Facultad–Carrera (composición):** El rombo negro se coloca junto a Facultad porque el caso indica que las carreras se dan de baja si cierra la facultad.
* **Carrera–Pensum:** El enunciado indica que cada carrera está formada por un pensum.
* **Pensum–Asignatura:** El pensum contiene asignaturas. Si se representa como composición, el rombo negro debe estar junto a Pensum; esta decisión debe coincidir con el diagrama final.
* **Asignatura–Asignatura (reflexiva):** Representa que una asignatura puede requerir otras previamente aprobadas. Se recomiendan multiplicidades `0..*` en ambos extremos.
* **Estudiante–RegistroMatricula:** Un estudiante puede tener varios registros; cada registro corresponde a un estudiante.
* **RegistroMatricula–CicloAcademico:** Un ciclo puede relacionarse con varios registros; cada registro corresponde a un ciclo.
* **RegistroMatricula–GrupoClase:** La matrícula detalla los grupos seleccionados y un grupo puede aparecer en varias matrículas.
* **GrupoClase–Docente:** Cada grupo tiene exactamente un docente; un docente puede impartir varios grupos.
* **GrupoClase–Aula (asociación simple):** El aula existe independientemente de los grupos, por lo que no se necesita composición ni agregación.

---

## 7. Justificación de la relación reflexiva de prerrequisitos

La asociación reflexiva conecta la clase `Asignatura` consigo misma porque una materia puede requerir que se hayan aprobado otras asignaturas antes de cursarla (por ejemplo, *Programación I*). 

La multiplicidad `0..*` en ambos extremos permite que una asignatura no tenga prerrequisitos o tenga varios, y que una asignatura sirva de prerrequisito para otras. Se recomienda nombrar los extremos `prerrequisito` y `asignaturaDependiente`.

Como validaciones adicionales, el sistema podría impedir que una asignatura sea su propio prerrequisito y detectar ciclos, si esas reglas se incorporan a los requerimientos.

---

## 8. Diccionario técnico

El diccionario técnico completo se entrega por separado en `docs/caso3_unicore.md`. Contiene el propósito de las diez clases, sus atributos y operaciones, la tabla de relaciones y multiplicidades, y las justificaciones de composición, asociación y prerrequisitos.
