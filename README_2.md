# Decisiones estratégicas en sistemas y tecnologías de la información
## Conceptos importantes:
<div align=center>
<img src="img/SI-TI.png" width="50%" alt="Sistemas de Información y Tecnologías de Información" >
</div>

<center>

| **Sistemas de Información** | **Tecnologías de Información** |
|:---------------------------:|:------------------------------:|
|      Son un conjunto formal de procesos que, operando sobre una colección de datos estructurados en función de las necesidades específicas del negocio, recopila, elabora y distribuye la información necesaria para la operación de la organización.      |     Son los recursos tecnológicos (hardware, software) que constituyen el soporte físico y lógico para el tratamiento automatizado de la información.     |
</center>

## Pirámide de decisiones

<div align=center>
<img src="img/piramide.png" width="100%" alt="Piramide de decisiones" >
</div>

- **Dirección operativa**

    Se encarga de que los procesos de negocio funcionen sin errores. La generación de los datos reflejan la operatividad del día a día.

- **Dirección táctica**

    Enfocada a la toma de decisiones a mediano plazo, la dirección táctica se encarga de la recopilación de la información interna para la toma de decisiones, planificación o control.

- **Dirección estratégica**
    
    Se encarga de la recopilación de la información **externa**, con el fin de generar estrategias que signifiquen un beneficio para la empresa.
    Quienes operan en esta capa son el director de tecnología y el director general.

## Integración de la infraestructura

```mermaid
flowchart TD
  A("Arquitectura Tecnológica") e1@--- B
  e1@{ animation: fast }
  B("Modelo de Datos") e2@--- C
  e2@{ animation: fast }
  C("Modelo de Procesos") e3@--- D("Modelo de Aplicaciones")
  e3@{ animation: fast }
```

La integración asegura que la tecnología no sea solo una herramienta, sino un pilar de la organización, donde cualquier cambio debe alinearse al modelo original y lograr un principio de consistencia.
