# Modelo-Entidad-Relacion.-Viveros
P3 de Administración y Diseño de bases de datos

Azael Santana Domínguez alu0101542119@ull.edu.es

Gabriel Gallardo Noda Alu0101633961@ull.edu.es

# Modelo Entidad/Relación — Tajinaste S.A.

## 1. Descripción de las entidades

### VIVERO

Representa cada uno de los viveros pertenecientes a la red de Tajinaste S.A.

**Atributos:**
- `georreferenciacion`: identifica la localización del vivero mediante su latitud y longitud.

### ZONA

Representa las diferentes zonas en las que se divide un vivero, como pueden ser una zona exterior, un almacén, etc.

**Atributos:**
- `georreferenciacion`: identifica la localización de la zona mediante su latitud y longitud.
- `tipo`: indica el tipo de zona, por ejemplo, exterior o almacén.

### PRODUCTO

Representa los productos comercializados por Tajinaste S.A.

**Atributos:**
- `id_producto`: identificador único del producto.
- `tipo_producto`: indica la categoría del producto, pudiendo ser planta, producto de jardinería o producto de decoración.

### EMPLEADO

Representa a los empleados de Tajinaste S.A.

**Atributos:**
- `id_empleado`: identificador único del empleado.

El resto de información relacionada con el puesto, las tareas y los períodos de trabajo se recoge mediante la relación de asignación, ya que estos datos pueden variar a lo largo del tiempo.

### CLIENTE

Representa a los clientes de Tajinaste S.A.

**Atributos:**
- `id_cliente`: identificador único del cliente.
- `miembro_tajinaste_plus`: indica mediante un valor Sí/No si el cliente pertenece al programa de fidelización Tajinaste Plus.
- `fecha_inicio_plus`: fecha en la que el cliente se incorporó al programa Tajinaste Plus. Será nula si el cliente no pertenece al programa.

### BONIFICACION

Representa las bonificaciones mensuales asignadas a los clientes que pertenecen al programa Tajinaste Plus.

Se considera una entidad débil, ya que una bonificación se identifica en función del cliente al que pertenece y del mes al que corresponde.

**Atributos:**
- `mes`: mes al que corresponde la bonificación.
- `volumen_compra`: volumen de compras realizado por el cliente durante ese mes.
- `bonificacion`: bonificación obtenida en función del volumen de compras.

La identificación de una bonificación se realiza mediante el cliente y el mes correspondiente.

### PEDIDO

Representa los pedidos realizados por los clientes de Tajinaste S.A.

**Atributos:**
- `id_pedido`: identificador único del pedido.
- `fecha`: fecha en la que se realiza el pedido.
- `tipo_pedido`: indica el tipo de pedido.

---

## 2. Descripción de los atributos y sus dominios

| Entidad | Atributo | Dominio / Descripción | Ejemplo |
|---|---|---|---|
| VIVERO | `georreferenciacion` | Latitud y longitud del vivero | `V001` |
| ZONA | `georreferenciacion` | Latitud y longitud de la zona | `28.4636, -16.2518` |
| ZONA | `tipo` | Tipo de zona del vivero | `Almacén` |
| PRODUCTO | `id_producto` | Identificador único del producto | `P025` |
| PRODUCTO | `tipo_producto` | Planta, jardinería o decoración | `Planta` |
| EMPLEADO | `id_empleado` | Identificador único del empleado | `E015` |
| CLIENTE | `id_cliente` | Identificador único del cliente | `C102` |
| CLIENTE | `miembro_tajinaste_plus` | Indica si pertenece al programa | `Sí` |
| CLIENTE | `fecha_inicio_plus` | Fecha de incorporación al programa | `10/03/2026` |
| BONIFICACION | `mes` | Mes al que corresponde la bonificación | `03/2026` |
| BONIFICACION | `volumen_compra` | Volumen de compras realizado durante el mes | `750 €` |
| BONIFICACION | `bonificacion` | Bonificación obtenida | `50 €` |
| PEDIDO | `id_pedido` | Identificador único del pedido | `PED1005` |
| PEDIDO | `fecha` | Fecha de realización del pedido | `15/04/2026` |
| PEDIDO | `tipo_pedido` | Tipo de pedido | `Venta` |

---

## 3. Descripción de las relaciones y cardinalidades

### VIVERO — COMPUESTO POR — ZONA

Representa la división de cada vivero en diferentes zonas.

**Cardinalidad: 1:N**

- Un vivero puede estar compuesto por varias zonas.
- Cada zona pertenece a un vivero.

Esto permite organizar los productos y empleados dentro de las diferentes zonas de cada vivero.

### ZONA — CONTIENE — PRODUCTO

Representa la asignación de productos a las diferentes zonas de los viveros.

**Cardinalidad: N:M**

- Una zona puede contener diferentes productos.
- Un producto puede estar asignado a diferentes zonas.

La relación tiene el atributo:

- `cantidad`: indica la cantidad disponible de ese producto en esa zona.

De esta forma se puede conocer el stock de cada producto en cada zona.

### EMPLEADO — DESTINADO A — VIVERO

Representa la asignación de los empleados a los viveros.

La asignación es histórica, ya que un empleado puede estar destinado a diferentes viveros en distintos períodos, pero no puede tener dos destinos simultáneamente.

**Atributos de la relación:**
- `fecha_inicio`: inicio del período de asignación.
- `fecha_fin`: final del período de asignación.
- `puesto`: puesto desempeñado por el empleado.

La relación permite conocer el historial de puestos y destinos de cada empleado.

### EMPLEADO - TRABAJA EN - ZONA

Indica la zona concreta en la que trabaja un empleado dentro del vivero al que está destinado.

**Cardinalidad: N:M**

- un empleado puede estar destinado a diferentes viveros a lo largo del tiempo
- un vivero puede tener varios empleados destinados.
- Un empleado no puede tener dos destinos simultáneamente.

La zona en la que trabaja el empleado debe corresponder al vivero al que está destinado en ese periodo.

### CLIENTE — RECIBE — BONIFICACION

Representa las bonificaciones mensuales que reciben los clientes pertenecientes al programa Tajinaste Plus.

**Cardinalidad: 1:N**

- Un cliente puede tener varias bonificaciones, correspondientes a diferentes meses.
- Cada bonificación pertenece a un único cliente.

Los clientes que no pertenecen a Tajinaste Plus no tendrán registros de bonificación.

### CLIENTE — REALIZA — PEDIDO

Representa los pedidos realizados por los clientes.

**Cardinalidad: 1:N**

- Un cliente puede realizar varios pedidos.
- Cada pedido pertenece a un único cliente.

La fecha del pedido permite determinar qué pedidos fueron realizados por un cliente después de su incorporación al programa Tajinaste Plus.

### EMPLEADO — GESTIONA — PEDIDO

Representa la gestión de los pedidos por parte de los empleados.

**Cardinalidad: 1:N**

- Un empleado puede gestionar varios pedidos.
- Cada pedido tiene un único empleado responsable.

Esta relación permite realizar el seguimiento de los pedidos gestionados por cada empleado y utilizar esta información como uno de los factores para analizar su productividad.

---

## 4. Restricciones semánticas

Además de las cardinalidades representadas en el modelo, se consideran las siguientes restricciones semánticas:

1. **Un empleado no puede tener dos destinos simultáneamente.**  
   Los períodos definidos mediante `fecha_inicio` y `fecha_fin` para las asignaciones de un mismo empleado no pueden solaparse.

2. **Solo los clientes pertenecientes a Tajinaste Plus reciben bonificaciones.**  
   Si `miembro_tajinaste_plus` es `No`, el cliente no tendrá registros asociados en `BONIFICACION`.

3. **La fecha de inicio de Tajinaste Plus es obligatoria para los miembros del programa.**  
   Si `miembro_tajinaste_plus` es `Sí`, `fecha_inicio_plus` debe estar informada. Si es `No`, `fecha_inicio_plus` será nula.

4. **Cada pedido tiene un único responsable.**  
   Un pedido debe estar asociado a exactamente un empleado mediante la relación `GESTIONA`.

5. **La cantidad de un producto se corresponde con una zona concreta.**  
   El atributo `cantidad` de `CONTIENE` representa el stock de un producto en una determinada zona, y no una cantidad global del producto.

6. **Las bonificaciones son mensuales.**  
   Para un mismo cliente no debe existir más de una bonificación para el mismo mes. Por ello, la identificación de `BONIFICACION` se basa en el cliente y el mes.

7. **Los pedidos utilizados para las campañas de Tajinaste Plus son los realizados por clientes pertenecientes al programa desde su fecha de incorporación.**  
   Esta información se puede obtener relacionando `CLIENTE` con `PEDIDO` y comparando la fecha del pedido con `fecha_inicio_plus`.

---

## 5. Consideraciones sobre la productividad

El enunciado indica que la productividad de las zonas y de los empleados debe ser objeto de seguimiento, pero no define una fórmula concreta para calcularla ni un atributo específico que deba almacenarse.

Por este motivo, no se añade una entidad independiente de `PRODUCTIVIDAD`.

La información necesaria para realizar este análisis queda recogida en el modelo mediante:

- La asignación histórica de los empleados a viveros y zonas.
- Las fechas de realización de los pedidos.
- Los pedidos gestionados por cada empleado.
- La información de stock de las zonas.

La productividad puede obtenerse posteriormente mediante consultas sobre estos datos, sin introducir información que no haya sido definida en el enunciado.
