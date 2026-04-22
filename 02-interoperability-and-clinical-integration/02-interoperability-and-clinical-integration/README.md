# 02. Interoperabilidad e integración clínica

## 1. Propósito de la capa

### 1.1. Qué cubre esta capa

Esta capa se centra en cómo los sistemas clínicos intercambian, exponen y encaminan la información para que los datos puedan moverse desde el lugar donde se generan hasta el lugar donde realmente se necesitan.

Si la capa anterior trataba de entender qué datos existen, en qué sistemas viven y qué calidad o limitaciones tienen, esta capa trata de cómo hacer que esa información circule de forma utilizable dentro del ecosistema clínico.

El objetivo no es describir una arquitectura ideal construida desde cero, sino entender cómo conectar sistemas hospitalarios reales de forma suficientemente robusta como para soportar analítica, alertas, soporte a la decisión o servicios basados en IA sin romper los flujos operativos.

### 1.2. Por qué la IA no puede vivir aislada

Un componente de IA puede ser técnicamente competente y, aun así, no aportar valor si permanece desconectado de los sistemas y procesos donde realmente ocurre la actividad clínica.

En la práctica, una IA útil en sanidad suele depender de su capacidad para recibir información desde sistemas existentes, procesarla a tiempo y devolver un resultado por un canal que encaje con el flujo clínico real.

Por eso, la interoperabilidad no es solo una cuestión técnica. Es una de las condiciones que determinan si un sistema de IA puede pasar de una demo a formar parte del trabajo habitual.

---

## 2. Qué significa interoperabilidad en entornos reales

### 2.1. Ecosistemas clínicos heterogéneos

Un hospital rara vez funciona como una única plataforma digital coherente. Lo habitual es convivir con múltiples sistemas incorporados a lo largo del tiempo para funciones distintas: historia clínica electrónica, laboratorio, farmacia, imagen, admisión, alta, aplicaciones departamentales, repositorios secundarios, etc.

Cada uno de estos sistemas puede manejar estructuras de datos distintas, identificadores diferentes, ritmos de actualización propios y mecanismos de integración no homogéneos.

Por eso, en la práctica, la interoperabilidad rara vez consiste en conectar dos sistemas modernos y limpios. Lo normal es intentar que la información circule entre componentes heterogéneos.

### 2.2. Convivencia con sistemas legacy

Una parte importante de la interoperabilidad hospitalaria sigue dependiendo de sistemas legacy y de patrones de integración establecidos hace años.

En muchos entornos, una parte del flujo clínico sigue apoyándose en mensajería HL7 v2.x, interfaces antiguas, conectores específicos, bases intermedias o integraciones desarrolladas para necesidades concretas, no pensando originalmente en casos de uso de IA.

Esto no las convierte en algo irrelevante. Al contrario: en muchos casos, esas piezas son precisamente la base real sobre la que deben construirse nuevos servicios.

### 2.3. Intercambio de datos frente a integración real

Enviar o recibir datos no equivale a integrar un proceso.

Un sistema puede recibir mensajes o exponer un endpoint y, aun así, no encajar de verdad en el flujo clínico. La integración real exige que la información llegue al lugar adecuado, en el momento adecuado, con el formato adecuado y de una forma que facilite la acción clínica en lugar de añadir fricción.

Por eso, la interoperabilidad no debería reducirse a la existencia de un estándar o de una interfaz. También implica ajuste al proceso, fiabilidad operativa y capacidad de sostener el intercambio útil en el tiempo.

### 2.4. Interoperabilidad técnica, semántica y de proceso

La interoperabilidad tiene varias dimensiones.

A nivel técnico, los sistemas deben poder intercambiar información mediante algún mecanismo. A nivel semántico, esa información debe significar lo mismo en ambos lados. Y a nivel de proceso, el intercambio debe encajar en el flujo de trabajo donde se utiliza.

Muchos problemas no aparecen porque no exista conexión, sino porque lo que se intercambia no se interpreta igual, no llega con el contexto necesario o no se incorpora bien al circuito clínico correspondiente.
