# Diccionario Técnico y Justificación de Diseño
**Caso 2: Plataforma E-Commerce Omnicanal ("GlobalMarket")**  
**Asignatura:** Lenguaje Unificado de Modelado (UML)  
**Institución:** Universidad Tecnológica de El Salvador (UTEC)  

---

## 1. Introducción y Propósito del Modelo
El presente documento describe la arquitectura y especificación técnica del diagrama de clases diseñado para la plataforma e-commerce de *GlobalMarket*. El objetivo del modelo es representar con rigor técnico el núcleo operativo de pedidos, gestión de clientes, procesamiento de pagos y la logística de despachos, garantizando alta cohesión y bajo acoplamiento en la estructura orientada a objetos.

---

## 2. Descripción de Entidades y Diccionario de Clases

### 2.1. Dominio de Clientes (`Cliente`, `ClienteCorporativo`, `ClienteIndividual`)
* **`Cliente` (Clase Abstracta):** Modela la abstracción general de un usuario comprador en la plataforma. Agrupa los atributos base compartidos por cualquier tipo de cliente.
  * **Atributos:**
    * `+ idCliente: String` – Identificador único del cliente.
    * `+ nombre: String` – Nombre principal o comercial.
    * `+ email: String` – Correo electrónico para notificaciones y cuenta.
    * `+ telefono: String` – Número telefónico de contacto.
    * `+ direccion: String` – Dirección principal de facturación/entrega.
* **`ClienteCorporativo` (Subclase):** Especialización de cliente para personas jurídicas / empresas.
  * **Atributos:**
    * `+ ruc: String` – Registro de identificación tributaria/comercial.
    * `+ razonSocial: String` – Nombre legal registral.
    * `+ limiteCredito: double` – Monto máximo otorgado para compras a crédito.
* **`ClienteIndividual` (Subclase):** Especialización de cliente para personas naturales.
  * **Atributos:**
    * `+ dui: String` – Documento Único de Identidad.
    * `+ apellido: String` – Apellidos del cliente.

### 2.2. Núcleo de Compras (`Pedido`, `LineaDeDetalle`, `Articulo`)
* **`Pedido`:** Representa una transacción u orden de compra dentro del e-commerce.
  * **Atributos:**
    * `+ idPedido: String` – Código único del pedido.
    * `+ fecha: DateTime` – Fecha y hora de creación.
    * `+ estado: String` – Estado actual (ej. *Pendiente*, *Pagado*, *Enviado*).
    * `+ total: double` – Monto total acumulado del pedido.
  * **Métodos:**
    * `+ calcularTotal(): double` – Suma los subtotales de las líneas de detalle asociadas.
* **`LineaDeDetalle`:** Elemento individual que compone un pedido, especificando la cantidad y subtotal de un artículo específico.
  * **Atributos:**
    * `+ cantidad: int` – Unidades compradas.
    * `+ precioUnitario: double` – Precio al momento de realizar la orden.
    * `+ subtotal: double` – Resultado del producto entre cantidad y precio unitario.
  * **Métodos:**
    * `+ calcularSubtotal(): double` – Procesa el valor total de la línea.
* **`Articulo`:** Producto del catálogo disponible para la venta.
  * **Atributos:**
    * `+ idArticulo: String` – Código del ítem.
    * `+ nombre: String` – Nombre comercial del producto.
    * `+ descripcion: String` – Detalle del producto.
    * `+ precio: double` – Precio de lista actual.
    * `+ stock: int` – Existencias en inventario.

### 2.3. Dominio de Transacciones (`Pago`, `Tarjeta`, `Transferencia`, `Criptomoneda`)
* **`Pago` (Clase Abstracta):** Abstracción general del procesamiento de fondos.
  * **Atributos:**
    * `+ idPago: String` – Identificador único de la transacción.
    * `+ monto: double` – Valor monetario procesado.
    * `+ fechaPago: DateTime` – Marca temporal de la transacción.
    * `+ estado: String` – Estado de la operación (*Aprobado*, *Rechazado*).
  * **Métodos:**
    * `+ procesarPago(): boolean` – Método abstracto definido para ser implementado según la pasarela.
* **Subclases de Pago (`Tarjeta`, `Transferencia`, `Criptomoneda`):**
  * Implementan las particularidades de cada pasarela (números de tarjeta, bancos emisores o direcciones de billeteras *blockchain*) y concretan la lógica de autorización del método `procesarPago()`.

### 2.4. Logística y Despacho (`Envio`)
* **`Envio`:** Modela el seguimiento del despacho físico del paquete gestionado por proveedores externos.
  * **Atributos:**
    * `+ idEnvio: String` – Identificador del paquete.
    * `+ numeroRastreo: String` – Guía de seguimiento (*tracking number*).
    * `+ empresaLogistica: String` – Proveedor logístico (ej. DHL, FedEx).
    * `+ estadoEnvio: String` – Estado (*En tránsito*, *Entregado*).
    * `+ fechaEstimadaEntrega: DateTime` – Estimación de arribo.
  * **Métodos:**
    * `+ actualizarEstado(nuevoEstado: String): void` – Registra cambios de trazabilidad.

---

## 3. Justificación Técnica de Decisiones de Diseño UML

### 3.1. Justificación de Composición Obligatoria (`Pedido` $\rightarrow$ `LineaDeDetalle`)
Se implementó una **relación de Composición (fuerte)** entre `Pedido` y `LineaDeDetalle` por las siguientes razones de negocio y arquitectura:
1. **Dependencia de Existencia:** Una `LineaDeDetalle` no posee sentido de negocio independiente dentro del sistema sin estar vinculada a una cabecera de `Pedido`.
2. **Ciclo de Vida Compartido:** Si un `Pedido` es destruido o anulado completamente del sistema de persistencia, todas las `LineaDeDetalle` asociadas deben eliminarse en cascada.

### 3.2. Justificación de Agregación / Asociación (`LineaDeDetalle` $\rightarrow$ `Articulo`)
A diferencia de la relación anterior, la vinculación con `Articulo` es una **Asociación simple**, ya que los artículos del inventario existen independientemente de si pertenecen o no a una orden de compra activa.

### 3.3. Justificación de Herencia y Polimorfismo en `Pago` y `Cliente`
* **Generalización de Clientes:** Permite la extensibilidad del sistema para incorporar futuros tipos de clientes (ej. *ClienteMayorista*) sin romper la relación existente con `Pedido`.
* **Polimorfismo en Pagos:** La clase abstracta `Pago` define la firma del método `procesarPago()`, permitiendo que el módulo de pedidos efectúe cobros de forma desacoplada sin importar el medio de pago seleccionado.