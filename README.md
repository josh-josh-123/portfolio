https://github.com/josh-josh-123/portfolio/blob/main/edificios.png?raw=true

👋 Profesional del sector financiero especializado en **productos y canales de pago** con un fuerte enfoque en **Ciencia de Datos**, estrategia y ejecución.

📍 Ciudad de México  
🔗 [LinkedIn](https://linkedin.com/in/jose-manuel-sanchez-hernandez-a6173932)  
📧 josemsh@yahoo.com  

---

## 🚀 Sobre mí

Cuento con amplia experiencia liderando productos de pago en banca empresarial y corporativa, combinando **visión de negocio**, **análisis de datos** y **gestión de equipos multidisciplinarios**.  
He trabajado en entornos altamente regulados y con plataformas globales, impulsando crecimiento sostenible, eficiencia operativa y mejoras en la experiencia del cliente.

Actualmente me estoy formando como **Data Scientist**, con interés particular en analítica aplicada a pagos, riesgo, comportamiento de clientes y uso de canales.

---

## 🧠 Áreas de especialidad

- Gestión de productos y canales de pago (SPEI, TEF, Cheques, Impuestos, Domiciliación, Cobranza referenciada)
- Planeación estratégica
- Ciencia de Datos aplicada a negocio
- Gestión y liderazgo de equipos
- Transformación y gestión del cambio

---

## 🛠️ Habilidades

- **Idiomas:** Español (nativo), Inglés (avanzado – TOEIC 930)
- **Gestión:** Desarrollo y ejecución de proyectos, metodologías ágiles
- **Data & Analytics:** Python, Pandas, NumPy, Power BI, Dashboards

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

## 📊 Proyecto 2: Planes de pago

![Proyecto 2](https://github.com/josh-josh-123/portfolio/blob/main/uso_celular.png?raw=true)

**Contexto del proyecto**  
Esta empresa de telefonía ofrece a sus clientes dos tarifas de prepago, Surf y Ultimate, con disponibilidad de 15GB y 30GB de datos, ya incluidos en la tarifa, respectivamente. El departamento comercial quiere saber cuál de las tarifas genera más ingresos para poder ajustar el presupuesto de publicidad. Se tiene la información en 5 tablas interrelacionadas: catálogo de clientes y plan contratado, catálogo de tarifas, consumo de llamadas, consumo de SMS y consumo de datos por cada cliente. 

**Análisis**  
- Uso de librerías: Pandas, Numpy, Matplotlib, Seaborn, Scipy, Math
- Uso de funciones y método Apply para calcular el ingreso mensual por cada usuario
- Aplicación de herramientas estadísticas: Boxplot, Histogramas, Pruebas de hipótesis

**Conclusiones principales**  

- Existe un segmento importante de usuarios que demandan mayor disponiblidad de internet en sus paquetes. Hoy están limitados a 15gb y 30gb respectivamente.

- 60% de los pagos que se reciben en Surf pagan extra por el uso de internet. Esto es bueno para la compañía, por recibir ingresos extra, pero existe un riesgo de que otra compañía competidora les ofrezca mejores planes y los perdamos.

- 35% de los pagos recibidos en Surf estan en el plan incorrecto. Son de usuarios intensivos de internet. Es bueno para la compañía que estos clientes paguen en exceso, sin embargo existe riesgo de que otra compañía les ofrezca un mejor paquete y los perdamos.

- Se recomienda a la compañía analizar la posibilidad de crear un nuevo paquete, dirigido al segmento de usuarios intensivos en el uso de internet, con un precio mensual aproximado a $45 con capacidad de navegar en internet hasta 30gb, a la vez ampliar la capacidad del plan Ultimate a 40gb. Con ello tendría opción de atender mejor a los clientes Surf y dar un plan que cubra bien las necesidades de Ultimate. El nuevo paquete debe promocionarse agresivamente para traer nuevos clientes, y debe usarse defensivamente para evitar que los clientes actuales se vayan.

🔗 **Notebook:**  
[Ver notebook Planes_de_pago](https://github.com/josh-josh-123/portfolio/blob/9e1bc0a2fd254484d60ac35b2d4b07df7a50fe1a/Planes_de_pago.md)

---

## 📊 Proyecto 3: Videojuegos

![Proyecto 3]( https://github.com/josh-josh-123/portfolio/blob/main/videojuegos_personajes.png?raw=true)

**Contexto del proyecto**  
Tienda online que vende videojuegos globalmente. Se deben identificar patrones que determinen si un juego tiene éxito o no. Esto permite detectar proyectos prometedores y planificar campañas publicitarias. Los datos provistos corresponden al año 2016. Se requiere completar un análisis que lleve a tomar decisiones de negocio en diciembre de 2016, para ser ejecutadas en 2017.
El dataset provisto contiene: Nombre del juego, plataforma, año de lanzamiento, género, rating de críticos especializados, rating de usuarios, ventas en región NA, región EU, región JP y otras regiones.

**Análisis**  
- Limpieza de datos y preparación de datos:
    - Ajustar nombre de columnas
    - Valores ausentes.
    - Valores nulos
    - Sustitución de valores
    - Agregar los datos de género, en una categoría mas amplia que permita su análisis.
- Uso de librerías: Pandas, Numpy, Matplotlib, Seaborn, Scipy, Math
- Aplicación de herramientas estadísticas: Boxplot, Boxplot grouped, Cálculo de correlación, Histogramas, Análisis de cuartiles, Pruebas de hipótesis, 
- Gráficas de dispersión, barras apiladas, barras separadas

**Conclusiones principales**  

- La información del año 2016 está incompleta, pero aun así es útil porque los datos, aunque parciales, parecen estar bien en cuanto a sus proporciones, con lo cual se obtiene información reciente valiosa para orientar las decisiones (lo confirman las gráficas 1, 2, 3, 4 y 12)

- La venta de videojuegos es un mercado que va la baja desde el año 2010. Se recomienda diversificar los ingresos con ventas de productos/servicios sustitutos o complementarios, que aprovechen los canales que ya tiene la empresa y vayan dirigidos a estos mismos clientes preferentemente (lo confirma la gráfica 4)

- La información que tenemos de ingresos por Ventas está vinculada al año en el que se publicó cada videojuego. Sabemos que lo mejor sería tener los ingresos de cada juego en cada año, pero al no disponer de ello, el análisis lo debemos hacer asumiendo que los ingresos de cada juego se dan en el año en que se publicaron (year_of_release), lo cual es una buena aproximación.

- El negocio de videojuegos tiene alta incertidumbre, debido a que los ingresos que genera provienen de un número muy limitado de títulos que tienen ventas muy altas. En contraste un gran porcentaje de los juegos publicados generan muy bajas ganancias (lo confirman las Gráficas 5, 6 y 7)

- Aun así se pueden tomar en cuenta algunos rasgos de la información para orientar los esfuerzos comerciales en los juegos que ofrecen la mayor probabilidad de éxito:

    - Plataforma: las plataformas dominantes son 2.Sony y 3.Microsoft (lo confirman las gráficas 11 y 13). Las consolas más dominantes son (en orden):
        - Norteamérica: PS4, XOne, X360, PS3 y PC
        - Europa: PS4, XOne, PS3, PC, y X360
        - Japón: PS4, PS3, PSV, 3DS, WiiU
    - Genero: El esfuerzo comercial debe orientarse hacia Action y Shooter por tener las ventas relevantes. Action tiende a decrecer y Shooter a mantenerse en su nivel. Los géneros Role-Playing y Sports siguen en nivel de importancia (lo confirma la gráfica 12)
    - Edad: La mayor probabilidad de éxito está en las opciones 5.Mature y 2.Everyone (lo confirma la gráfica 15)
    - Juegos multiplataforma: Los juegos que se lanzan en plataformas múltiples suelen tener mayor éxito que los de plataforma individual. Se confirma que los fabricantes pueden identificar con anticipación los juegos de alto potencial (lo confirma la gráfica 10).
    - Score de la crítica y usuarios: Si bien tenemos una correlación positiva de la crítica a las ventas, el índice de correlación es cercano a cero. La crítica es un aspecto a considerar, pero no es un aspecto determinante. (lo confirman las gráficas 8 y 9)

🔗 **Notebook:**  
[Ver notebook Videojuegos]( https://github.com/josh-josh-123/portfolio/blob/847806ba29e51ff8c6429d7b4ecc025906d25141/Videojuegos.md)

---

📌 *Nota: Los proyectos se encuentran en constante evolución conforme avanzo en el bootcamp y desarrollo nuevas habilidades en Data Science.*
