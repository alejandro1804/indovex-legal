# IndovexApp — Pendientes legales

**Documentos alcanzados:** Términos y Condiciones de Uso · Política de Privacidad y Protección de Datos Personales
**Versiones vigentes al generar este documento:** T&C v1.4 · Privacidad v1.6 (Julio 2026)
**Última actualización de este documento:** 15 de julio de 2026

> Este archivo lleva el registro de lo que falta resolver en los documentos legales, por qué falta, y qué hay que averiguar antes de poder cerrarlo. Se actualiza cada vez que se publica una versión nueva.

---

## Cómo leer este documento

| Prioridad | Significado |
|---|---|
| **P1** | Bloquea el go-live comercial o declara algo factualmente falso |
| **P2** | Debe resolverse antes del primer cliente pago |
| **P3** | Mejora de robustez; puede esperar |

| Estado | Significado |
|---|---|
| 🔴 Bloqueado | Falta un dato externo (contador, proveedor, abogado) |
| 🟡 Pendiente | Se puede resolver, falta decisión o tiempo |
| 🟢 Listo para redactar | Decisión tomada, sólo falta escribirlo |

---

## 1. Plazo de conservación de logs técnicos con contenido personal

- **Documento:** Privacidad, cláusula 5.3
- **Prioridad:** P2
- **Estado:** 🔴 Bloqueado

**Situación.** La v1.1 declaraba que los registros técnicos y de auditoría con contenido personal se conservaban hasta 12 meses. Esa frase desapareció en v1.5 y no volvió en v1.6. Hoy la política describe el ciclo de vida de los datos del Cliente (60 días tras baja/suspensión) pero **no declara ningún plazo para los logs técnicos** (IP, tráfico de sesión, logs de autenticación) descritos en la cláusula 2.3.

**Por qué importa.** El artículo 7 de la LPDP exige que los datos no se conserven más allá de lo necesario para la finalidad. Una política que describe un tratamiento sin declarar su plazo es un hueco visible en una inspección de la URCDP.

**Por qué está bloqueado.** El plazo no lo define IndovexApp: lo definen los proveedores que efectivamente retienen esos logs.

- **Supabase** — logs de autenticación y de la plataforma. Verificar retención en su documentación y DPA.
- **Cloudflare** — logs de proxy/CDN con IP de origen. Verificar retención según el plan contratado (los planes gratuitos y de pago difieren).

Declarar "12 meses" por inercia de la v1.1 sería inventar un número: si Cloudflare retiene otra cosa, la política estaría afirmando algo falso sobre un tratamiento que IndovexApp no controla.

**Qué hay que hacer.**

1. Revisar la documentación de retención de logs de Supabase (docs + DPA).
2. Revisar la documentación de retención de logs de Cloudflare para el plan en uso.
3. Redactar el plazo de forma que refleje la realidad: puede ser un plazo concreto o una remisión a las políticas de los encargados, pero debe ser verdadero.

**Redacción tentativa (a validar con los datos reales):**

> Los registros técnicos de conexión descritos en la cláusula 2.3 son generados y conservados por los proveedores de infraestructura conforme a sus propias políticas de retención, por plazos que no exceden los [X] meses. El Proveedor no conserva copias adicionales de dichos registros más allá de lo necesario para la investigación de incidentes de seguridad.

---

## 2. Retención de datos de facturación (obligación fiscal)

- **Documento:** Privacidad, cláusula 5.3
- **Prioridad:** P2
- **Estado:** 🔴 Bloqueado — requiere consulta al contador

**Situación.** Tanto los T&C (cláusula 10) como la Privacidad (cláusula 1) declaran que, respecto de los datos contractuales y de facturación (razón social, RUT, domicilio, contacto de quien contrata), **IndovexApp actúa como responsable del tratamiento**, no como encargado. Esa distinción es correcta y está bien redactada.

La consecuencia es que esos datos **no** siguen el ciclo de vida de 60 días de la cláusula 5.3: están sujetos a plazos de conservación fiscales que sobreviven a la baja del cliente. La política actual no declara nada al respecto, lo que crea una contradicción implícita: un cliente que lee 5.3 puede concluir que a los 60 días de la baja no queda rastro suyo, cuando la factura emitida debe conservarse por obligación legal.

**Por qué está bloqueado.** El plazo surge del Código Tributario y de la normativa de DGI, y tiene matices que no conviene resolver de memoria:

- Plazo general de prescripción tributaria y su extensión en supuestos de defraudación.
- Cómo aplica bajo el régimen de **Literal E**.
- Reparto de responsabilidad de conservación entre IndovexApp y el **habilitador CFE externo** que emite los comprobantes: quién conserva qué, en qué formato, y por cuánto tiempo.

**Qué preguntarle al contador.**

1. ¿Por cuántos años debo conservar los comprobantes emitidos y la documentación de respaldo bajo Literal E?
2. ¿La conservación que hace el habilitador CFE me libera de conservar copia propia, o debo mantenerla igual?
3. ¿Los datos de contacto del cliente (más allá de razón social y RUT) forman parte de la documentación que debo conservar, o puedo depurarlos antes?

**Redacción tentativa (a validar):**

> **Datos de facturación.** Los datos de la relación contractual y de facturación, respecto de los cuales el Proveedor actúa como responsable del tratamiento, se conservan por el plazo que impone la normativa tributaria uruguaya, con independencia de la baja del Cliente. Vencido dicho plazo, se procede a su eliminación. Esta conservación responde a una obligación legal y prevalece sobre los plazos de la presente cláusula.

**Nota de diseño.** Cuando esto se resuelva, hay que verificar que la implementación real (la purga automatizada) excluya `pagos_suscripcion` y los datos fiscales de `empresas` del borrado a los 60 días. Si el código purga todo y la política dice otra cosa, el problema pasa de ser documental a ser real.

---

## 3. Precios en los documentos

- **Documento:** T&C, cláusula 5.1
- **Prioridad:** P3
- **Estado:** 🟡 Pendiente

**Situación.** Los T&C remiten a indovexapp.com para los precios vigentes, lo cual es correcto y flexible (evita reeditar los T&C ante cada cambio de precio). Los precios están definidos: Starter $450 UYU y Pro $1.100 UYU mensuales, sin IVA por el régimen Literal E.

**Qué verificar.** Que la página pública efectivamente muestre esos precios y la aclaración del régimen fiscal. Si los T&C remiten a una página que no los tiene, la remisión queda vacía.

**Decisión ya tomada:** no hardcodear precios en los T&C. Mantener la remisión.

---

## 4. Aviso de modificación de T&C — mecanismo de implementación

- **Documento:** T&C, cláusula 13; Privacidad, cláusula 10
- **Prioridad:** P2
- **Estado:** 🟡 Pendiente

**Situación.** Ambos documentos comprometen notificar cambios con **15 días de anticipación** por correo electrónico o aviso en plataforma. Ese compromiso hoy no tiene implementación técnica: no existe un mecanismo de aviso in-app ni un envío masivo de correo a clientes.

**Por qué importa.** Es una obligación autoimpuesta. Con cero clientes pagos el incumplimiento es teórico, pero desde el primer cliente pasa a ser real, y una modificación de T&C sin el aviso previo es inoponible al cliente.

**Qué hay que hacer.**

- Definir el mecanismo: banner in-app, correo transaccional, o ambos.
- Registrar en base la aceptación de cada versión de T&C por cliente (fecha, versión, usuario que acepta). Esto además alimenta la trazabilidad ALCOA+.
- Considerar una tabla `aceptaciones_legales` (`empresa_id`, `documento`, `version`, `usuario_id`, `fecha`, `ip`).

**Relación con otros pendientes.** Esto es prerequisito real para poder publicar cualquier versión futura una vez que haya clientes. Conviene resolverlo antes que los pendientes #1 y #2, porque son justamente los que van a disparar la próxima modificación.

---

## 5. Registro ante la URCDP

- **Documento:** transversal
- **Prioridad:** P1
- **Estado:** 🟡 Pendiente

**Situación.** La Ley 18.331 exige inscribir las bases de datos personales ante la URCDP. IndovexApp trata datos personales tanto en carácter de encargado (datos de los usuarios del cliente) como de responsable (datos contractuales y de facturación).

**Por qué es P1.** La política publicada declara cumplimiento de la LPDP y remite a la URCDP como organismo de control. Declarar cumplimiento sin haber completado la inscripción es una inconsistencia expuesta públicamente.

**Qué hay que hacer.** Completar la inscripción. Existe un documento de trabajo previo (`Registro_URCDP_IndovexApp.docx`) que conviene revisar y actualizar contra las versiones v1.4/v1.6, ya que fue redactado contra versiones anteriores.

---

## 6. Revisión por abogado

- **Documento:** ambos
- **Prioridad:** P2
- **Estado:** 🔴 Bloqueado

**Situación.** Ambos documentos llevan al pie la advertencia de que no constituyen asesoramiento legal y recomiendan revisión profesional. Esa nota es honesta, pero es un parche: mientras siga ahí, los documentos son un borrador publicado.

**Puntos que conviene que un abogado mire con atención.**

- **Tope de responsabilidad** (T&C cláusula 8): el límite de 3 meses de suscripción es agresivo. Verificar que sea sostenible frente a la normativa de orden público, especialmente si algún cliente pudiera calificar como consumidor.
- **Política de no reembolso** (T&C cláusula 5.3): ya fue suavizada con la salvedad de disposición legal imperativa. Confirmar que la redacción actual es suficiente.
- **Cláusula territorial** (T&C cláusula 3): confirmar que la facultad de rechazar altas y dar de baja por domicilio fuera de Uruguay no genera exposición por discriminación o por incumplimiento contractual sobrevenido.
- **Renuncia de fuero** (T&C cláusula 14): verificar su validez, sobre todo frente a clientes que pudieran ser consumidores.
- **Doble rol responsable/encargado**: validar que el desdoblamiento declarado en Privacidad cláusula 1 y T&C cláusula 10 esté bien construido bajo la LPDP.

---

## 7. Contrato de encargado de tratamiento con clientes

- **Documento:** nuevo — no existe
- **Prioridad:** P3
- **Estado:** 🟡 Pendiente

**Situación.** Los T&C declaran que IndovexApp actúa como encargado del tratamiento respecto de los datos que el cliente ingresa. Bajo la LPDP, esa relación normalmente se instrumenta en un acuerdo específico que detalla instrucciones, medidas de seguridad, subencargados y devolución/eliminación de datos.

Hoy esa relación se apoya únicamente en la cláusula 10 de los T&C. Para un cliente en industria farmacéutica o alimentaria sujeto a auditorías de LATU, INAC o MGAP, es previsible que soliciten un documento específico.

**Qué hay que hacer.** Evaluar si conviene un anexo de tratamiento de datos incorporado por referencia a los T&C, o un documento separado a firmar. Consultar en la misma sesión que la revisión general por abogado (pendiente #6).

**Ventaja de anticiparlo.** Es un diferenciador comercial: un cliente farmacéutico que pide el DPA y lo recibe al día siguiente ve un proveedor serio.

---

## 8. Verificación de coherencia código ↔ documentos

- **Documento:** transversal
- **Prioridad:** P2
- **Estado:** 🟡 Pendiente

**Situación.** Los documentos hacen afirmaciones verificables sobre el comportamiento del sistema. Cada una es una promesa que la implementación debe cumplir.

| Afirmación | Ubicación | ¿Implementado? |
|---|---|---|
| Exportación completa en CSV y PDF antes de la baja | T&C 12.1 | Verificar cobertura real |
| Conservación de 60 días tras baja/suspensión | T&C 12.2, Priv. 5.3 | Verificar que exista el proceso |
| Purga definitiva vencido el plazo | T&C 12.3 | Verificar que exista el proceso |
| Anonimización del audit log tras la purga | T&C 12.3, Priv. 5.3 | Verificar |
| Aviso final antes de la purga | T&C 12.3 | Verificar |
| Audit log inmutable | Priv. 5.2 | ✅ `fn_proteger_audit_log` |
| Aislamiento por empresa a nivel de BD | Priv. 5.2 | ✅ RLS |
| Contraseñas con hash bcrypt | Priv. 5.2 | ✅ Supabase Auth |
| No se almacenan datos de tarjetas | T&C 5.2 | ✅ tokenización MercadoPago |
| Teléfono usado para WhatsApp sólo si el usuario lo habilita | Priv. 6 | Verificar que el opt-in sea explícito |

**Riesgo específico:** la cláusula de anonimización del audit log promete suprimir nombres, correos, teléfonos e IPs manteniendo la traza. El audit log está protegido contra modificación por `fn_proteger_audit_log`. Hay que confirmar que existe una vía legítima para ejecutar esa anonimización sin romper la inmutabilidad, o la promesa no es cumplible.

---

## Resumen de bloqueos

| # | Pendiente | P | Depende de |
|---|---|---|---|
| 5 | Registro URCDP | P1 | Trámite propio |
| 1 | Plazo logs técnicos | P2 | Docs de Supabase y Cloudflare |
| 2 | Retención fiscal de facturación | P2 | Contador |
| 4 | Mecanismo de aviso de cambios | P2 | Desarrollo propio |
| 6 | Revisión por abogado | P2 | Abogado |
| 8 | Coherencia código ↔ documentos | P2 | Auditoría propia |
| 3 | Precios en la web | P3 | Verificación propia |
| 7 | DPA con clientes | P3 | Abogado (junto con #6) |

**Orden sugerido:** #4 primero (habilita publicar todo lo demás sin incumplir el propio compromiso de aviso) → #1 y #3 (verificaciones baratas) → #2 y #6 con las consultas externas → #5 → #7 y #8.

---

## Bitácora de versiones

| Fecha | T&C | Privacidad | Cambios |
|---|---|---|---|
| Jun 2026 | v1.1 | v1.1 | Versión inicial documentada |
| Jun 2026 | v1.3 | v1.5 | Desdoblamiento responsable/encargado; plazos a 60 días; no reembolso suavizado; encargados nominados; cláusula de datos técnicos e IP |
| Jul 2026 | **v1.4** | **v1.6** | **T&C:** nueva cláusula 3 (ámbito territorial: sólo Uruguay, con facultad de rechazo y baja); renumeración de cláusulas 3→15. **Privacidad:** RUT marcado como opcional (2.1); Twilio removido, sólo Meta Platforms (6); referencia al ámbito territorial en el preámbulo |