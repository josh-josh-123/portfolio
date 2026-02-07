👋 Profesional del sector financiero especializado en **productos y canales de pago** con un fuerte enfoque en **Ciencia de Datos**, estrategia y ejecución.

📍 Ciudad de México  
🔗 [LinkedIn](https://linkedin.com/in/jose-manuel-sanchez-hernandez-a6173932)  
📧 josemsh@yahoo.com  

---

## 🚀 Sobre mí

Cuento con amplia experiencia liderando productos de pago en banca global y fintech, combinando **visión de negocio**, **análisis de datos** y **gestión de equipos multidisciplinarios**.  
He trabajado en entornos altamente regulados y con plataformas globales, impulsando crecimiento sostenible, eficiencia operativa y mejoras en la experiencia del cliente.

Actualmente me estoy formando como **Data Scientist**, con interés particular en analítica aplicada a pagos, riesgo, comportamiento de clientes y optimización de canales.

---

## 🧠 Áreas de especialidad

- Productos y canales de pago (SPEI, TEF, Cheques, Impuestos, Domiciliación)
- Gestión de producto y planeación estratégica
- Ciencia de Datos aplicada a negocio
- Sistemas de pago y operación bancaria
- Banca corporativa, empresarial y microfinanzas
- Gestión y liderazgo de equipos
- Transformación y gestión del cambio

---

## 🛠️ Habilidades

- **Idiomas:** Español (nativo), Inglés (avanzado – TOEIC 930)
- **Gestión:** Desarrollo y ejecución de proyectos, metodologías ágiles
- **Data & Analytics:** Python, Pandas, NumPy, Power BI, visualización, KPI’s
- **Negocio:** Diseño de incentivos, modelos operativos, optimización de canales

---

## 💼 Experiencia profesional

### J.P. Morgan  
**Vicepresidente de Producto** — Ciudad de México  
*03/2023 – 07/2025*

- Liderazgo integral de productos de pago (Cheques, SPEI, TEF, Pago de Impuestos).
- Crecimiento sostenido a doble dígito en volumen transaccional.
- Gestión del cambio en iniciativas estratégicas globales bajo metodologías ágiles.
- Seguimiento de KPI’s, continuidad operativa y cumplimiento regulatorio.

---

### Grupo Gentera  
**Subdirector de Gestión de Canales** — Ciudad de México  
*07/2016 – 06/2023*

- Expansión de red de corresponsales en +2,000 tiendas usando analítica de datos.
- Diseño de métricas, incentivos y modelos de *best-next-action*.
- Gestión de cartera en riesgo manteniendo morosidad < 3%.

---

### HSBC México  
**Director de Producto, Canales y Riesgo Operativo** — Ciudad de México  
*07/1995 – 06/2016*

- Gestión de productos de pago electrónico para banca empresarial y corporativa.
- Liderazgo de proyectos regionales en México y Latinoamérica.

---

## 🎓 Formación

- **Data Scientist Certificate** — TripleTen *(2025–2026, en proceso)*  
- **MBA** — IPADE  
- **Ingeniería Industrial** — Universidad Panamericana  

---

# 📂 Proyectos de Data Science

## Introducción

Esta sección presenta una selección de proyectos desarrollados durante mi **bootcamp de Data Science**, enfocados en el uso de datos para resolver problemas reales de negocio.  
Los proyectos abarcan análisis exploratorio, modelado predictivo y visualización, utilizando **Python, librerías de análisis de datos y notebooks reproducibles**.

---

## 📊 Proyecto 1: Supermercado

![Proyecto 1](https://github.com/josh-josh-123/portfolio/blob/main/img_super_612x612.jpg?raw=true)

**Contexto del proyecto**  
Un supermercado online busca entender el comportamiento transaccional de sus clientes para mejorar la recompra e identificar productos de baja demanda. Se tiene la información en 5 tablas interrelacionadas: pedidos recibidos, artículos solicitados en cada pedido, catálogo de productos, catálogo de pasillos y catálogo de departamentos.

**Análisis**  
- Importación de datasets de alto volumen (4.5 millones de registros) con tabuladores atípicos.
- Uso de librerías: Pandas, Numpy y Matplotlib
- Limpieza y exploración de datos, incluyendo:
- Registros duplicados
- Valores nulos
- Valores vacíos
- Valores inconsistentes (mismo significado con distinta sintaxis)
- Valores incorrectos

**Conclusiones principales**  
- Los pedidos están concentrados entre las 9 y las 16hrs. Pudiera ser un cuello de botella a analizar. De confirmarse que se trata de un cuello de botella podría analizarse la opción de dar descuento en horarios fuera de este periodo.

- No hay diferencia significativa en el número de pedidos que se tienen en los distintos días de la semana.
- 10,000 clientes no esperaron ni un día para hacer su siguiente pedido. Es indicativo de que una vez realizado su pedido, se dieron cuenta que se les había olvidado solicitar algún artículo. Se recomienda analizar estos casos con mayor profundidad para que el mensajero sólo haga un viaje a la casa del cliente (consolidar los pedidos)
- No funciona bien el contador de días transcurridos desde el último pedido. Sólo cuenta bien hasta el día 30 días. Esto es un tema urgente a corregir;  esta variable es muy relevante para identificar clientes que dejaron de usar el servicio, y sirve para detonar llamadas o promociones para averiguar el motivo y recuperar clientes.
- El pedido mas común es de 5 artículos, con 31,923 casos observados. Esto nos habla de que muchos clientes usan el servicio para pedir productos ocasionales, suponemos que el "super grande" lo hacen físicamente en una tienda.
- El top 20 de artículos que vuelven a pedirse con mayor frecuencia incluye sólo productos frescos y la mayor parte de ellos del tipo orgánico. Este tipo de artículos también son los primeros que el cliente pone en su carrito de compras. Esta claro que nuestra calidad y catálogo de productos frescos (en particular orgánicos) es una ventaja competitiva.
🔗 **Notebook:**  
[Ver notebook_supermercado](https://github.com/josh-josh-123/portfolio/blob/f597b0eb1e28eb75ca4e42140b6657cdddafa529/Supermercado.ipynb)

---

## 📉 Proyecto 2: Predicción de Riesgo de Morosidad en Microcréditos

![Proyecto 2](https://images.unsplash.com/photo-1554224155-8d04cb21cd6c)

**Contexto del proyecto**  
Una institución de microfinanzas desea anticipar qué clientes tienen mayor probabilidad de caer en mora para tomar acciones preventivas.

**Análisis**  
- Análisis exploratorio de variables financieras y demográficas  
- Ingeniería de variables  
- Entrenamiento de modelos de clasificación (Logistic Regression, Random Forest)  

**Conclusiones principales**  
- Variables de comportamiento reciente son más predictivas que datos demográficos  
- El modelo permite priorizar acciones de cobranza preventiva  
- Mejora potencial en la eficiencia operativa del área de riesgo  

🔗 **Notebook:**  
https://github.com/usuario/prediccion-morosidad/blob/main/notebook.ipynb

---

## 📈 Proyecto 3: Optimización de Red de Comercios con Data Analytics

![Proyecto 3](https://images.unsplash.com/photo-1543286386-713bdd548da4)

**Contexto del proyecto**  
Optimizar la cobertura geográfica y el desempeño de una red de comercios corresponsales en zonas no bancarizadas.

**Análisis**  
- Análisis geoespacial y de desempeño por tienda  
- Segmentación por ciclo de vida del comercio  
- Definición de reglas de *best-next-action*  

**Conclusiones principales**  
- Identificación de tiendas clave para expansión o retención  
- Mejor asignación de recursos comerciales  
- Base analítica para toma de decisiones estratégicas  

🔗 **Notebook:**  
https://github.com/usuario/optimizacion-red-comercios/blob/main/notebook.ipynb

---

📌 *Nota: Los proyectos se encuentran en constante evolución conforme avanzo en el bootcamp y desarrollo nuevas habilidades en Data Science.*
