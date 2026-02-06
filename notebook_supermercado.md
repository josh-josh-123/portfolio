----

# PROYECTO: SUPERMERCADO

# Introducción

Instacart es una plataforma de entregas de comestibles donde la clientela puede registrar un pedido y hacer que se lo entreguen, similar a Uber Eats y Door Dash.
El conjunto de datos que te hemos proporcionado tiene modificaciones del original. Redujimos el tamaño del conjunto para que tus cálculos se hicieran más rápido e introdujimos valores ausentes y duplicados. Tuvimos cuidado de conservar las distribuciones de los datos originales cuando hicimos los cambios.

Debes completar tres pasos. Para cada uno de ellos, escribe una breve introducción que refleje con claridad cómo pretendes resolver cada paso, y escribe párrafos explicatorios que justifiquen tus decisiones al tiempo que avanzas en tu solución.  También escribe una conclusión que resuma tus hallazgos y elecciones.


## Diccionario de datos

Hay cinco tablas en el conjunto de datos, y tendrás que usarlas todas para hacer el preprocesamiento de datos y el análisis exploratorio de datos. A continuación se muestra un diccionario de datos que enumera las columnas de cada tabla y describe los datos que contienen.

- `instacart_orders.csv`: cada fila corresponde a un pedido en la aplicación Instacart.
    - `'order_id'`: número de ID que identifica de manera única cada pedido.
    - `'user_id'`: número de ID que identifica de manera única la cuenta de cada cliente.
    - `'order_number'`: el número de veces que este cliente ha hecho un pedido.
    - `'order_dow'`: día de la semana en que se hizo el pedido (0 si es domingo).
    - `'order_hour_of_day'`: hora del día en que se hizo el pedido.
    - `'days_since_prior_order'`: número de días transcurridos desde que este cliente hizo su pedido anterior.
- `products.csv`: cada fila corresponde a un producto único que pueden comprar los clientes.
    - `'product_id'`: número ID que identifica de manera única cada producto.
    - `'product_name'`: nombre del producto.
    - `'aisle_id'`: número ID que identifica de manera única cada categoría de pasillo de víveres.
    - `'department_id'`: número ID que identifica de manera única cada departamento de víveres.
- `order_products.csv`: cada fila corresponde a un artículo pedido en un pedido.
    - `'order_id'`: número de ID que identifica de manera única cada pedido.
    - `'product_id'`: número ID que identifica de manera única cada producto.
    - `'add_to_cart_order'`: el orden secuencial en el que se añadió cada artículo en el carrito.
    - `'reordered'`: 0 si el cliente nunca ha pedido este producto antes, 1 si lo ha pedido.
- `aisles.csv`
    - `'aisle_id'`: número ID que identifica de manera única cada categoría de pasillo de víveres.
    - `'aisle'`: nombre del pasillo.
- `departments.csv`
    - `'department_id'`: número ID que identifica de manera única cada departamento de víveres.
    - `'department'`: nombre del departamento.
from google.colab import drive
drive.mount('/content/drive')
# Paso 1. Descripción de los datos

Lee los archivos de datos (`/datasets/instacart_orders.csv`, `/datasets/products.csv`, `/datasets/aisles.csv`, `/datasets/departments.csv` y `/datasets/order_products.csv`) con `pd.read_csv()` usando los parámetros adecuados para leer los datos correctamente. Verifica la información para cada DataFrame creado.


## Plan de solución

Escribe aquí tu plan de solución para el Paso 1. Descripción de los datos.

Abrir los archivos para ver que tipo de separadores tienen.

Importar las librerias y revisar en forma general su contenido.

Confirmar cuales son las variables que tienen en común, cuales son los campos llave y el tipo de datos que incluyen.

Tener una idea clara del volúmen de datos, de la existencia de datos nulos y tipo de datos.

Entender la lógica de los datos desde el punto de vista de negocio y establecer una primera apreciación general del tipo de análisis que se puede hacer de estos datos.




```python
# importar librerías
import pandas as pd
import numpy as np
from matplotlib import pyplot as plt
```


```python
# leer conjuntos de datos en los DataFrames
df_orders = pd.read_csv('/content/instacart_orders.csv',sep=';')
df_order_products = pd.read_csv('/content/order_products.csv',sep=';')
df_products = pd.read_csv('/content/products.csv',sep=';')
df_aisles = pd.read_csv('/content/aisles.csv',sep=';')
df_departments = pd.read_csv('/content/departments.csv',sep=';')

```

<div class="alert alert-block alert-success">
<b>Comentario de Revisor         </b> <a class="tocSkip"></a>

Correcto. Bien hecho al importar la data usando el parámetro `sep=;"` para que la lectura de los datos se pueda realizar correctamente.
    

</div>


```python
# mostrar información del DataFrame
df_orders.info(show_counts=True)
print()
df_orders.sample(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 478967 entries, 0 to 478966
    Data columns (total 6 columns):
     #   Column                  Non-Null Count   Dtype  
    ---  ------                  --------------   -----  
     0   order_id                478967 non-null  int64  
     1   user_id                 478967 non-null  int64  
     2   order_number            478967 non-null  int64  
     3   order_dow               478967 non-null  int64  
     4   order_hour_of_day       478967 non-null  int64  
     5   days_since_prior_order  450148 non-null  float64
    dtypes: float64(1), int64(5)
    memory usage: 21.9 MB
    
    





  <div id="df-42481d7a-f058-47e4-91ce-a19fe4e2d8b2" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>order_id</th>
      <th>user_id</th>
      <th>order_number</th>
      <th>order_dow</th>
      <th>order_hour_of_day</th>
      <th>days_since_prior_order</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>73820</th>
      <td>2408170</td>
      <td>72458</td>
      <td>4</td>
      <td>3</td>
      <td>14</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>191017</th>
      <td>1579781</td>
      <td>110227</td>
      <td>5</td>
      <td>5</td>
      <td>7</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>327430</th>
      <td>727862</td>
      <td>21572</td>
      <td>7</td>
      <td>1</td>
      <td>9</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>423064</th>
      <td>707889</td>
      <td>74940</td>
      <td>1</td>
      <td>3</td>
      <td>23</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>133735</th>
      <td>449716</td>
      <td>74764</td>
      <td>26</td>
      <td>2</td>
      <td>15</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>246579</th>
      <td>3377931</td>
      <td>141825</td>
      <td>35</td>
      <td>0</td>
      <td>8</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>389415</th>
      <td>1703143</td>
      <td>155414</td>
      <td>16</td>
      <td>4</td>
      <td>8</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>345122</th>
      <td>852301</td>
      <td>85943</td>
      <td>8</td>
      <td>3</td>
      <td>19</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>241981</th>
      <td>1642650</td>
      <td>29569</td>
      <td>28</td>
      <td>5</td>
      <td>12</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>35599</th>
      <td>336414</td>
      <td>26343</td>
      <td>11</td>
      <td>0</td>
      <td>16</td>
      <td>21.0</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-42481d7a-f058-47e4-91ce-a19fe4e2d8b2')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-42481d7a-f058-47e4-91ce-a19fe4e2d8b2 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-42481d7a-f058-47e4-91ce-a19fe4e2d8b2');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-06d0d11a-017a-433a-92e9-8afae535f630">
      <button class="colab-df-quickchart" onclick="quickchart('df-06d0d11a-017a-433a-92e9-8afae535f630')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-06d0d11a-017a-433a-92e9-8afae535f630 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




El separador ";" usado, importó correctamente los distintos campos.
No se tienen datos nulos.

Los campos que deben tener tipo "int64" lo tienen.

Debemos analizar con mas profundidad porque el campo days_since_prior_order se importó con tipo float64 y cambiarlo a int64 si es posible.

El campo llave es order_id. No debería estar duplicado.

**Tipo de analísis que podríamos hacer:**  No nos proporcionan información "absoluta" de fechas de compra de los pedidos, en su lugar tenemos información "relativa" a la hora en que se realizan las compras, el día de la semana, tiempo transcurrido desde la compra previa. Con estos datos el análisis se debe enfocar en encontrar en identificar grupos de compradores con patrones en común.
Sería interesante poder preguntar el periodo en el que se generaron los datos; parece que la variable days_since_prior_order nos puede dar una idea.


```python
# mostrar información del DataFrame
df_order_products.info(show_counts=True)
print()
df_order_products.sample(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4545007 entries, 0 to 4545006
    Data columns (total 4 columns):
     #   Column             Non-Null Count    Dtype  
    ---  ------             --------------    -----  
     0   order_id           4545007 non-null  int64  
     1   product_id         4545007 non-null  int64  
     2   add_to_cart_order  4544171 non-null  float64
     3   reordered          4545007 non-null  int64  
    dtypes: float64(1), int64(3)
    memory usage: 138.7 MB
    
    





  <div id="df-d7696478-76ca-44a5-b285-dde38822f2c7" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>order_id</th>
      <th>product_id</th>
      <th>add_to_cart_order</th>
      <th>reordered</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>181405</th>
      <td>2517437</td>
      <td>913</td>
      <td>9.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3931285</th>
      <td>1451606</td>
      <td>27521</td>
      <td>13.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2361495</th>
      <td>1532338</td>
      <td>26346</td>
      <td>13.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4308183</th>
      <td>3052782</td>
      <td>29487</td>
      <td>12.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3594601</th>
      <td>3095537</td>
      <td>21903</td>
      <td>9.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3426813</th>
      <td>1456151</td>
      <td>21847</td>
      <td>5.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>874482</th>
      <td>2264250</td>
      <td>13852</td>
      <td>26.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>76902</th>
      <td>3398946</td>
      <td>38371</td>
      <td>9.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>807461</th>
      <td>1758038</td>
      <td>37158</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>400472</th>
      <td>414909</td>
      <td>28836</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-d7696478-76ca-44a5-b285-dde38822f2c7')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-d7696478-76ca-44a5-b285-dde38822f2c7 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-d7696478-76ca-44a5-b285-dde38822f2c7');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-3c339fc1-0e50-469d-a512-94a91b6116ff">
      <button class="colab-df-quickchart" onclick="quickchart('df-3c339fc1-0e50-469d-a512-94a91b6116ff')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-3c339fc1-0e50-469d-a512-94a91b6116ff button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




El separador ";" usado, importó correctamente los distintos campos.

Es un dataframe muy grande, que tiene mas de 4.5 mill de registros.

El campo add_to_cart_order tiene datos nulos que debemos revisar cual es su mejor tratamiento. El campo add_to_cart_order tiene tipo float64 lo cual parece irregular y debe investigarse con mas profundidad  y cambiarlo a int64 si es posible. El resto de los campos que deben tener tipo "int64" lo tienen correctamente.

No hay un campo llave único. Sin embargo la combinación de los campos order_id, product_id y add_to_cart_order no debería estar duplicada.

**Tipo de analísis que podríamos hacer:** No nos dan información de los ingresos en $. Con los datos provistos el análisis puede enfocarse en identificar los productos mas vendidos, menos vendidos. No viene un campo que indique el número de unidades compradas de un mismo producto en una misma orden, pero supongo que podríamos obtener este importante dato usando el campo add_to_cart_order. Adicionalmente, con el campo 'reordered' podríamos analizar qué tan abiertos estan los clientes a pedir nuevos productos.


```python
# mostrar información del DataFrame
df_products.info()
print()
df_products.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 49694 entries, 0 to 49693
    Data columns (total 4 columns):
     #   Column         Non-Null Count  Dtype 
    ---  ------         --------------  ----- 
     0   product_id     49694 non-null  int64 
     1   product_name   48436 non-null  object
     2   aisle_id       49694 non-null  int64 
     3   department_id  49694 non-null  int64 
    dtypes: int64(3), object(1)
    memory usage: 1.5+ MB
    
    





  <div id="df-2a75f41e-9b7d-467e-887a-4f85447cfb46" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>product_id</th>
      <th>product_name</th>
      <th>aisle_id</th>
      <th>department_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Chocolate Sandwich Cookies</td>
      <td>61</td>
      <td>19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>All-Seasons Salt</td>
      <td>104</td>
      <td>13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Robust Golden Unsweetened Oolong Tea</td>
      <td>94</td>
      <td>7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Smart Ones Classic Favorites Mini Rigatoni Wit...</td>
      <td>38</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Green Chile Anytime Sauce</td>
      <td>5</td>
      <td>13</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Dry Nose Oil</td>
      <td>11</td>
      <td>11</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>Pure Coconut Water With Orange</td>
      <td>98</td>
      <td>7</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>Cut Russet Potatoes Steam N' Mash</td>
      <td>116</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>Light Strawberry Blueberry Yogurt</td>
      <td>120</td>
      <td>16</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>Sparkling Orange Juice &amp; Prickly Pear Beverage</td>
      <td>115</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-2a75f41e-9b7d-467e-887a-4f85447cfb46')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-2a75f41e-9b7d-467e-887a-4f85447cfb46 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-2a75f41e-9b7d-467e-887a-4f85447cfb46');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-43e2b2b9-ddbc-4333-8bc5-0148524d2a40">
      <button class="colab-df-quickchart" onclick="quickchart('df-43e2b2b9-ddbc-4333-8bc5-0148524d2a40')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-43e2b2b9-ddbc-4333-8bc5-0148524d2a40 button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




El separador ";" usado, importó correctamente los distintos campos.

El campo product_name tiene datos nulos que debemos revisar cual es su mejor tratamiento.

El campo product_name tiene tipo object lo cual parece correcto. El resto de los campos que deben tener tipo "int64" lo tienen correctamente.

El campo llave es product_id, no debería estar duplicado.

**Tipo de analísis que podríamos hacer:** Es un catálogo. No parece necesario hacer un análisis con esta información. Puede servir para complementar otros análisis.


```python
# mostrar información del DataFrame
df_aisles.info()
print()
df_aisles.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 134 entries, 0 to 133
    Data columns (total 2 columns):
     #   Column    Non-Null Count  Dtype 
    ---  ------    --------------  ----- 
     0   aisle_id  134 non-null    int64 
     1   aisle     134 non-null    object
    dtypes: int64(1), object(1)
    memory usage: 2.2+ KB
    
    





  <div id="df-08aa1be2-d2cb-4ad1-90ab-63551854b3be" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>aisle_id</th>
      <th>aisle</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>prepared soups salads</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>specialty cheeses</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>energy granola bars</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>instant foods</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>marinades meat preparation</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>other</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>packaged meat</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>bakery desserts</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>pasta sauce</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>kitchen supplies</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-08aa1be2-d2cb-4ad1-90ab-63551854b3be')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-08aa1be2-d2cb-4ad1-90ab-63551854b3be button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-08aa1be2-d2cb-4ad1-90ab-63551854b3be');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-0d346f2e-efc2-4f02-a6d5-64aa32941e3b">
      <button class="colab-df-quickchart" onclick="quickchart('df-0d346f2e-efc2-4f02-a6d5-64aa32941e3b')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-0d346f2e-efc2-4f02-a6d5-64aa32941e3b button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




El separador ";" usado, importó correctamente los distintos campos. No se tienen datos nulos y el tipo de datos parece correcto.

**Tipo de analísis que podríamos hacer:** Es un catálogo. No parece necesario hacer un análisis con esta información. Puede servir para complementar otros análisis.


```python
# mostrar información del DataFrame
df_departments.info()
print()
df_departments.head(10)
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 21 entries, 0 to 20
    Data columns (total 2 columns):
     #   Column         Non-Null Count  Dtype 
    ---  ------         --------------  ----- 
     0   department_id  21 non-null     int64 
     1   department     21 non-null     object
    dtypes: int64(1), object(1)
    memory usage: 468.0+ bytes
    
    





  <div id="df-6e865613-1b12-4e60-8cfb-d20e1000d8a1" class="colab-df-container">
    <div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>department_id</th>
      <th>department</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>frozen</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>other</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>bakery</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>produce</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>alcohol</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>international</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>beverages</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>pets</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>dry goods pasta</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>bulk</td>
    </tr>
  </tbody>
</table>
</div>
    <div class="colab-df-buttons">

  <div class="colab-df-container">
    <button class="colab-df-convert" onclick="convertToInteractive('df-6e865613-1b12-4e60-8cfb-d20e1000d8a1')"
            title="Convert this dataframe to an interactive table."
            style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960">
    <path d="M120-120v-720h720v720H120Zm60-500h600v-160H180v160Zm220 220h160v-160H400v160Zm0 220h160v-160H400v160ZM180-400h160v-160H180v160Zm440 0h160v-160H620v160ZM180-180h160v-160H180v160Zm440 0h160v-160H620v160Z"/>
  </svg>
    </button>

  <style>
    .colab-df-container {
      display:flex;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    .colab-df-buttons div {
      margin-bottom: 4px;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

    <script>
      const buttonEl =
        document.querySelector('#df-6e865613-1b12-4e60-8cfb-d20e1000d8a1 button.colab-df-convert');
      buttonEl.style.display =
        google.colab.kernel.accessAllowed ? 'block' : 'none';

      async function convertToInteractive(key) {
        const element = document.querySelector('#df-6e865613-1b12-4e60-8cfb-d20e1000d8a1');
        const dataTable =
          await google.colab.kernel.invokeFunction('convertToInteractive',
                                                    [key], {});
        if (!dataTable) return;

        const docLinkHtml = 'Like what you see? Visit the ' +
          '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
          + ' to learn more about interactive tables.';
        element.innerHTML = '';
        dataTable['output_type'] = 'display_data';
        await google.colab.output.renderOutput(dataTable, element);
        const docLink = document.createElement('div');
        docLink.innerHTML = docLinkHtml;
        element.appendChild(docLink);
      }
    </script>
  </div>


    <div id="df-94f9cb2e-6995-4fb2-a3c2-24e1800d49fc">
      <button class="colab-df-quickchart" onclick="quickchart('df-94f9cb2e-6995-4fb2-a3c2-24e1800d49fc')"
                title="Suggest charts"
                style="display:none;">

<svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
     width="24px">
    <g>
        <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zM9 17H7v-7h2v7zm4 0h-2V7h2v10zm4 0h-2v-4h2v4z"/>
    </g>
</svg>
      </button>

<style>
  .colab-df-quickchart {
      --bg-color: #E8F0FE;
      --fill-color: #1967D2;
      --hover-bg-color: #E2EBFA;
      --hover-fill-color: #174EA6;
      --disabled-fill-color: #AAA;
      --disabled-bg-color: #DDD;
  }

  [theme=dark] .colab-df-quickchart {
      --bg-color: #3B4455;
      --fill-color: #D2E3FC;
      --hover-bg-color: #434B5C;
      --hover-fill-color: #FFFFFF;
      --disabled-bg-color: #3B4455;
      --disabled-fill-color: #666;
  }

  .colab-df-quickchart {
    background-color: var(--bg-color);
    border: none;
    border-radius: 50%;
    cursor: pointer;
    display: none;
    fill: var(--fill-color);
    height: 32px;
    padding: 0;
    width: 32px;
  }

  .colab-df-quickchart:hover {
    background-color: var(--hover-bg-color);
    box-shadow: 0 1px 2px rgba(60, 64, 67, 0.3), 0 1px 3px 1px rgba(60, 64, 67, 0.15);
    fill: var(--button-hover-fill-color);
  }

  .colab-df-quickchart-complete:disabled,
  .colab-df-quickchart-complete:disabled:hover {
    background-color: var(--disabled-bg-color);
    fill: var(--disabled-fill-color);
    box-shadow: none;
  }

  .colab-df-spinner {
    border: 2px solid var(--fill-color);
    border-color: transparent;
    border-bottom-color: var(--fill-color);
    animation:
      spin 1s steps(1) infinite;
  }

  @keyframes spin {
    0% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
      border-left-color: var(--fill-color);
    }
    20% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    30% {
      border-color: transparent;
      border-left-color: var(--fill-color);
      border-top-color: var(--fill-color);
      border-right-color: var(--fill-color);
    }
    40% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-top-color: var(--fill-color);
    }
    60% {
      border-color: transparent;
      border-right-color: var(--fill-color);
    }
    80% {
      border-color: transparent;
      border-right-color: var(--fill-color);
      border-bottom-color: var(--fill-color);
    }
    90% {
      border-color: transparent;
      border-bottom-color: var(--fill-color);
    }
  }
</style>

      <script>
        async function quickchart(key) {
          const quickchartButtonEl =
            document.querySelector('#' + key + ' button');
          quickchartButtonEl.disabled = true;  // To prevent multiple clicks.
          quickchartButtonEl.classList.add('colab-df-spinner');
          try {
            const charts = await google.colab.kernel.invokeFunction(
                'suggestCharts', [key], {});
          } catch (error) {
            console.error('Error during call to suggestCharts:', error);
          }
          quickchartButtonEl.classList.remove('colab-df-spinner');
          quickchartButtonEl.classList.add('colab-df-quickchart-complete');
        }
        (() => {
          let quickchartButtonEl =
            document.querySelector('#df-94f9cb2e-6995-4fb2-a3c2-24e1800d49fc button');
          quickchartButtonEl.style.display =
            google.colab.kernel.accessAllowed ? 'block' : 'none';
        })();
      </script>
    </div>

    </div>
  </div>




El separador ";" usado, importó correctamente los distintos campos. No se tienen datos nulos y el tipo de datos parece correcto.

El campo llave es department_id, no debería estar duplicado.

**Tipo de analísis que podríamos hacer:** Es un catálogo. No parece necesario hacer un análisis con esta información. Puede servir para complementar otros análisis.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor          </b> <a class="tocSkip"></a>


Correcto, muy bien con el uso de `info()` para revisar las filas de los dataframes. Haces bien en utilizar el parámetro `show_counts=True`, ya que si no se pone esto y el dataframe es muy grande, no se mostrará el detalle de todas las filas.


</div>

## Conclusiones

Escribe aquí tus conclusiones intermedias sobre el Paso 1. Descripción de los datos.

En forma general podemos decir que los datos parecen tener buena calidad, siendo necesario sólo algunos procesos de preparación:
1. Ajustar el tipo de datos. Los campos df_orders['days_since_prior_order']   y   df_order_products['add_to_cart_order']   tienen tipo float64, cuando suponemos que lo correcto sería int64.
2. Eliminar o sustituir o estandarizar los valores nulos en: df_orders['days_since_prior_order'],   df_order_products['add_to_cart_order'] y df_products['product_name']
3. Revisar/corregir posible duplicidad de registros.
4. Los títulos de columnas estan bien.
5. Se obtiene una primer impresión del tipo de análisis que podríamos hacer:
5.1. Productos no vendidos: products y order_products, para identificar si el catálogo de productos disponibles resulta demasiado grande.
5.2. Tamaño de pedidos: instacar_orders y order_products para verificar si existe correlación entre frecuencia de pedidos y volumen
5.3 Identificar grupos de compradores con patrones en común. Sería interesante poder preguntar el periodo en el que se generaron los datos; parece que la variable days_since_prior_order nos puede dar una idea.
5.4 Identificar los productos mas vendidos, menos vendidos. No viene un campo que indique el número de unidades compradas de un mismo producto en una misma orden, pero supongo que podríamos obtener este importante dato usando el campo add_to_cart_order.
5.5. Adicionalmente, con el campo 'reordered' podríamos analizar qué tan abiertos estan los clientes a pedir nuevos productos.


# Paso 2. Preprocesamiento de los datos

Preprocesa los datos de la siguiente manera:

- Verifica y corrige los tipos de datos (por ejemplo, asegúrate de que las columnas de ID sean números enteros).
- Identifica y completa los valores ausentes.
- Identifica y elimina los valores duplicados.

Asegúrate de explicar qué tipos de valores ausentes y duplicados encontraste, cómo los completaste o eliminaste y por qué usaste esos métodos. ¿Por qué crees que estos valores ausentes y duplicados pueden haber estado presentes en el conjunto de datos?

## Plan de solución

Escribe aquí tu plan para el Paso 2. Preprocesamiento de los datos.

Ajustar el tipo de datos. Los campos df_orders['days_since_prior_order'] y df_order_products['add_to_cart_order'] tienen tipo float64, cuando suponemos que lo correcto sería int64.

Eliminar o sustituir o estandarizar los valores nulos en: df_orders['days_since_prior_order'], df_order_products['add_to_cart_order'] y df_products['product_name'].

Revisar/corregir posible duplicidad de registros.

## Encuentra y elimina los valores duplicados (y describe cómo tomaste tus decisiones).

### `orders` data frame


```python
# Revisa si hay pedidos duplicados
print(df_orders['order_id'].nunique())    # El campo 'order_id' es llave. No debería duplicarse. Pero el resultado 478,952 confirma que SÍ hay duplicados.

```

    478952
    

¿Tienes líneas duplicadas? Si sí, ¿qué tienen en común?


```python
# Basándote en tus hallazgos,
# Verifica todos los pedidos que se hicieron el miércoles a las 2:00 a.m.
print( df_orders[   (df_orders['order_hour_of_day'] == 2) & (df_orders['order_dow'] == 3)   ].sort_values(by='order_id') )   # No arroja datos concluyentes
print()
print("Valores duplicados en todo el DataFrame: ",df_orders.duplicated().sum())           # confirma que hay 15 valores duplicados en el DataFrame
print()
print("Valores duplicados campo 'order_id': ", df_orders['order_id'].duplicated().sum())  # confirma que hay 15 valores duplicados en el campo llave 'order_id'
print()
print( df_orders[   df_orders['order_id'].duplicated()])  # confirma que todos los duplicados son del miércoles a las 2am.
print()
print( df_orders[   df_orders['order_id']== 794638])      # Vemos un ejemplo de un registro duplicado
print()
print( df_orders[   df_orders['order_id']== 191])         # Vemos un ejemplo de un registro no duplicado. Confirma que no todos los pedidos del mie 2am se duplicaron

```

            order_id  user_id  order_number  order_dow  order_hour_of_day  \
    452265       191   176417            44          3                  2   
    468324    222962    54979            59          3                  2   
    247867    238782   196224             6          3                  2   
    417106    248760   204961            25          3                  2   
    328565    264348    34806             6          3                  2   
    ...          ...      ...           ...        ...                ...   
    97378    3226444   149996             3          3                  2   
    416198   3275652   169225             7          3                  2   
    415975   3286161    77320             9          3                  2   
    457013   3384021    14881             6          3                  2   
    178465   3389820    21703             2          3                  2   
    
            days_since_prior_order  
    452265                     6.0  
    468324                     3.0  
    247867                     3.0  
    417106                    15.0  
    328565                     5.0  
    ...                        ...  
    97378                     23.0  
    416198                    30.0  
    415975                     8.0  
    457013                    30.0  
    178465                    11.0  
    
    [121 rows x 6 columns]
    
    Valores duplicados en todo el DataFrame:  15
    
    Valores duplicados campo 'order_id':  15
    
            order_id  user_id  order_number  order_dow  order_hour_of_day  \
    145574    794638    50898            24          3                  2   
    223105   2160484   107525            16          3                  2   
    230807   1918001   188546            14          3                  2   
    266232   1782114   106752             1          3                  2   
    273805   1112182   202304            84          3                  2   
    284038   2845099    31189            11          3                  2   
    311713   1021560    53767             3          3                  2   
    321100    408114    68324             4          3                  2   
    323900   1919531   191501            32          3                  2   
    345917   2232988    82565             1          3                  2   
    371905    391768    57671            19          3                  2   
    394347    467134    63189            21          3                  2   
    411408   1286742   183220            48          3                  2   
    415163   2282673    86751            49          3                  2   
    441599   2125197    14050            48          3                  2   
    
            days_since_prior_order  
    145574                     2.0  
    223105                    30.0  
    230807                    16.0  
    266232                     NaN  
    273805                     6.0  
    284038                     7.0  
    311713                     9.0  
    321100                    18.0  
    323900                     7.0  
    345917                     NaN  
    371905                    10.0  
    394347                     2.0  
    411408                     4.0  
    415163                     2.0  
    441599                     3.0  
    
            order_id  user_id  order_number  order_dow  order_hour_of_day  \
    99462     794638    50898            24          3                  2   
    145574    794638    50898            24          3                  2   
    
            days_since_prior_order  
    99462                      2.0  
    145574                     2.0  
    
            order_id  user_id  order_number  order_dow  order_hour_of_day  \
    452265       191   176417            44          3                  2   
    
            days_since_prior_order  
    452265                     6.0  
    

¿Qué sugiere este resultado?

Hay 15 pedidos duplicados que deben eliminarse.


```python
# Elimina los pedidos duplicados
df_orders = df_orders.drop_duplicates(subset='order_id').reset_index(drop=True)

```


```python
# Vuelve a verificar si hay filas duplicadas
print("Valores duplicados en todo el DataFrame: ",df_orders.duplicated().sum())

```

    Valores duplicados en todo el DataFrame:  0
    


```python
# Vuelve a verificar únicamente si hay IDs duplicados de pedidos
print("Valores duplicados campo 'order_id': ", df_orders['order_id'].duplicated().sum())
```

    Valores duplicados campo 'order_id':  0
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

Identifique que había valores duplicados usando el campo llave order_id. Confirmé que eran 15, todos ellos el mismo día mie y hora 2am. Confirmé que alunos pedidos en esta mismo día y hora no estaban duplicados. Eliminé los registros duplicados y actualice los índices.

<div class="alert alert-block alert-success">
<b>Comentario de Reviewer         </b> <a class="tocSkip"></a>
    
Bien hecho, dado que todos los duplicados coincidian en el horario y día, es probable que se haya debido a un problema momentaneo en el registro de la data. En un entorno profesional, sería importante revisar que este problema no afecte a muchos registros y ver con el área responsable si esto es un problema conocido o no.
</div>

### `products` data frame


```python
# Verifica si hay filas totalmente duplicadas
print("Valores duplicados en todo el DataFrame: ",df_products.duplicated().sum())
```

    Valores duplicados en todo el DataFrame:  0
    


```python
# Revisa únicamente si hay ID de productos duplicados
print("Valores duplicados campo 'product_id': ", df_products['product_id'].duplicated().sum())  # confirma que no hay valores duplicados en en el campo llave 'product_id'
```

    Valores duplicados campo 'product_id':  0
    


```python
# Revisa únicamente si hay nombres duplicados de productos (convierte los nombres a letras mayúsculas para compararlos mejor)
print("Duplicados sin ajustar letras mayúsculas: ",df_products['product_name'].duplicated().sum())                # confirma 1257 valores product_name duplicados
print()
print("Duplicados ajustando a letras mayúsculas: ",df_products['product_name'].str.upper().duplicated().sum())    # confirma 1361 valores product_name duplicados
print()
print("Registros que tienen el duplicidad en el campo 'product_name'")
print()
print(df_products[df_products['product_name'].str.upper().duplicated()])     # algunos de los valores product_name duplicados son NaN y otros son texto válido.
print()
print(df_products[df_products['product_name'].str.upper() == 'HIGH PERFORMANCE ENERGY DRINK'])  # Ejemplo de nombre duplicado ajustando todo a mayúsculas.
```

    Duplicados sin ajustar letras mayúsculas:  1257
    
    Duplicados ajustando a letras mayúsculas:  1361
    
    Registros que tienen el duplicidad en el campo 'product_name'
    
           product_id                                     product_name  aisle_id  \
    71             72                                              NaN       100   
    109           110                                              NaN       100   
    296           297                                              NaN       100   
    416           417                                              NaN       100   
    436           437                                              NaN       100   
    ...           ...                                              ...       ...   
    49689       49690                    HIGH PERFORMANCE ENERGY DRINK        64   
    49690       49691                    ORIGINAL PANCAKE & WAFFLE MIX       130   
    49691       49692  ORGANIC INSTANT OATMEAL LIGHT MAPLE BROWN SUGAR       130   
    49692       49693                           SPRING WATER BODY WASH       127   
    49693       49694                          BURRITO- STEAK & CHEESE        38   
    
           department_id  
    71                21  
    109               21  
    296               21  
    416               21  
    436               21  
    ...              ...  
    49689              7  
    49690             14  
    49691             14  
    49692             11  
    49693              1  
    
    [1361 rows x 4 columns]
    
           product_id                   product_name  aisle_id  department_id
    22540       22541  High Performance Energy Drink        64              7
    49689       49690  HIGH PERFORMANCE ENERGY DRINK        64              7
    


```python
# Revisa si hay nombres duplicados de productos no faltantes
print("Duplicados que no son NaN, sin ajustar letras mayúsculas: ",df_products[df_products['product_name'].notna()]['product_name'].duplicated().sum())
#print()
print("Duplicados que no son NaN, ajustando nombres a letras mayúsculas: ",df_products[df_products['product_name'].notna()]['product_name'].str.upper().duplicated().sum())
df_product_id_dup = df_products[df_products['product_name'].notna()]['product_name'].str.upper().duplicated()
print(df_product_id_dup)
#print(df_products[df_product_id_dup])
```

    Duplicados que no son NaN, sin ajustar letras mayúsculas:  0
    Duplicados que no son NaN, ajustando nombres a letras mayúsculas:  104
    0        False
    1        False
    2        False
    3        False
    4        False
             ...  
    49689     True
    49690     True
    49691     True
    49692     True
    49693     True
    Name: product_name, Length: 48436, dtype: bool
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

No existen registros con product_id duplicados y eso es muy bueno porque éste es el campo llave.

En el caso de valores produc_name nulos, podemos dejarlos así, sin afectar el análisis. El producto existe porque tiene un product_id válido y único, pero falto incluír el nombre del producto y ello no impide que podamos hacer el análisis basándonos en el product_id.

En el caso de valores product_name repetidos, podemos dejarlos así, sin afectar el análisis. El producto existe porque tiene un product_id válido y único, pero todo indica que se trata de dos productos que son muy similares y coincidieron en la descrpción y ello no impide que podamos hacer el análisis basándonos en el product_id.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor           </b> <a class="tocSkip"></a>

Muy bien. En este caso, no es anormal encontrar nombres duplicados y no es necesario eliminarlos. Tienen diferente id, por lo tanto, es posible que sean diferentes versiones de un mismo producto.
</div>

### `departments` data frame


```python
# Revisa si hay filas totalmente duplicadas
print(df_departments.duplicated().sum())

```

    0
    


```python
# Revisa únicamente si hay IDs duplicadas de departamentos
print(df_departments['department_id'].duplicated().sum())     # No hay valores duplicados en el campo department_id
print(df_departments['department'].duplicated().sum())        # No hay valores duplicados en el campo department
```

    0
    0
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

Se confirma que no hay duplicidad en valores relevantes ni en registros.

### `aisles` data frame


```python
# Revisa si hay filas totalmente duplicadas
print(df_aisles.duplicated().sum())
```

    0
    


```python
# Revisa únicamente si hay IDs duplicadas de pasillos
print(df_aisles['aisle_id'].duplicated().sum())     # No hay valores duplicados en el campo aisle_id
print(df_aisles['aisle'].duplicated().sum())        # No hay valores duplicados en el campo aisle
```

    0
    0
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

Se confirma que no hay duplicidad en valores relevantes ni en registros.

### `order_products` data frame


```python
# Revisa si hay filas totalmente duplicadas
print(df_order_products.duplicated().sum())
```

    0
    


```python
# Vuelve a verificar si hay cualquier otro duplicado engañoso
     # Sería incorrecto que haya registros que coincidan en tener los mismos valores en order_id, product_id y add_to_cart_order
col1=df_order_products['order_id']
col2=df_order_products['product_id']
col3=df_order_products['add_to_cart_order']
print("Registros duplicados tomando en cuenta los campos order_id, product_id y add_to_cart_order : ", pd.concat([ col1,col2,col3] ,axis='columns').duplicated().sum() )
```

    Registros duplicados tomando en cuenta los campos order_id, product_id y add_to_cart_order :  0
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

Se confirma que no hay duplicidad en valores relevantes ni en registros. En este caso no existe un campo llave único. La llave es la combinación de 3 campos que tuve que concatenar, para descartar una posible duplicidad.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor        </b> <a class="tocSkip"></a>

Buen trabajo en esta sección con la revisión de duplicados

</div>

## Encuentra y elimina los valores ausentes

Al trabajar con valores duplicados, pudimos observar que también nos falta investigar valores ausentes:

* La columna `'product_name'` de la tabla products.
* La columna `'days_since_prior_order'` de la tabla orders.
* La columna `'add_to_cart_order'` de la tabla order_productos.

### `products` data frame


```python
# Encuentra los valores ausentes en la columna 'product_name'
print(df_products['product_name'].isna().sum())
print(df_products[df_products['product_name'].isna()])
```

    1258
           product_id product_name  aisle_id  department_id
    37             38          NaN       100             21
    71             72          NaN       100             21
    109           110          NaN       100             21
    296           297          NaN       100             21
    416           417          NaN       100             21
    ...           ...          ...       ...            ...
    49552       49553          NaN       100             21
    49574       49575          NaN       100             21
    49640       49641          NaN       100             21
    49663       49664          NaN       100             21
    49668       49669          NaN       100             21
    
    [1258 rows x 4 columns]
    

Describe brevemente cuáles son tus hallazgos.

Los valores ausentes parecen estar relacionados con el pasillo 100 y el departamento 21.


```python
#  ¿Todos los nombres de productos ausentes están relacionados con el pasillo con ID 100?
print(df_products[(df_products['product_name'].isna()) & (df_products['aisle_id']== 100)])
# Si todos los producto ausentes son de pasillo 100. Dado que al hacer la consulta de registros con nombre=NaN y pasillo=100, se obtiene la misma cifra 1258.

```

           product_id product_name  aisle_id  department_id
    37             38          NaN       100             21
    71             72          NaN       100             21
    109           110          NaN       100             21
    296           297          NaN       100             21
    416           417          NaN       100             21
    ...           ...          ...       ...            ...
    49552       49553          NaN       100             21
    49574       49575          NaN       100             21
    49640       49641          NaN       100             21
    49663       49664          NaN       100             21
    49668       49669          NaN       100             21
    
    [1258 rows x 4 columns]
    

Describe brevemente cuáles son tus hallazgos.

Primero descubrimos que había 1258 registros con valor NaN en el product_name, y pudimos ver en el sample que todos eran de pasillo 100. Luego hicimos la consulta de todos los registros con product_name=NaN y además pasillo=100, dando como resultado la misma cifra de 1258. Con lo cual se concluye que no hay otros valores de pasillo distintos a 100 que puedan tener product_name igual a NaN.


```python
# ¿Todos los nombres de productos ausentes están relacionados con el departamento con ID 21?
print(df_products[(df_products['product_name'].isna()) & (df_products['department_id']== 21)])
# Si todos los producto ausentes son de pasillo 100. Dado que al hacer la consulta de registros con nombre=NaN y pasillo=100, se obtiene la misma cifra 1258.
```

           product_id product_name  aisle_id  department_id
    37             38          NaN       100             21
    71             72          NaN       100             21
    109           110          NaN       100             21
    296           297          NaN       100             21
    416           417          NaN       100             21
    ...           ...          ...       ...            ...
    49552       49553          NaN       100             21
    49574       49575          NaN       100             21
    49640       49641          NaN       100             21
    49663       49664          NaN       100             21
    49668       49669          NaN       100             21
    
    [1258 rows x 4 columns]
    

Describe brevemente cuáles son tus hallazgos.

Primero descubrimos que había 1258 registros con valor NaN en el product_name, y pudimos ver en el sample que todos eran de department_id=21. Luego hicimos la consulta de todos los registros con product_name=NaN y además department_id=21, dando como resultado la misma cifra de 1258. Con lo cual se concluye que no hay otros valores de department_id distintos a 21 que puedan tener product_name igual a NaN.


```python
# Usa las tablas department y aisle para revisar los datos del pasillo con ID 100 y el departamento con ID 21.
print(df_aisles[df_aisles['aisle_id']== 100])
print()
print(df_departments[df_departments['department_id']== 21])
```

        aisle_id    aisle
    99       100  missing
    
        department_id department
    20             21    missing
    

Describe brevemente cuáles son tus hallazgos.

En el catálogo de pasillos df_aisles, en el índice 99, se tienen los valores: aisle_id=100 y aisle='missing'.

En el catálogo de departamentos df_departments, en el índice 20, se tienen los valores: department_id=21 y department='missing'.

El nombre 'missing' esta generando el error en ambos casos.


```python
# Completa los nombres de productos ausentes con 'Unknown'
df_aisles['aisle'] = df_aisles['aisle'].replace('missing', 'Unknown')
df_departments['department'] = df_departments['department'].replace('missing', 'Unknown')
print(df_aisles[df_aisles['aisle_id']== 100])
print()
print(df_departments[df_departments['department_id']== 21])
```

        aisle_id    aisle
    99       100  Unknown
    
        department_id department
    20             21    Unknown
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

A partir de la revisión de nombres de producto nulos en df_products, identificamos que el pasillo 100 y el departamento 21 estaban generando un problema. Investigando el asunto se descubrió que en los catálogos de pasillos df_aisles y departamentos df_departments se tenía un valor inválido, mismo que fue ajustado.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor        </b> <a class="tocSkip"></a>

Muy bien, debe haber sido un error específico al registrar los datos de ese pasillo y departamento
</div>

### `orders` data frame


```python
# Encuentra los valores ausentes
print("Número de Valores ausentes en df_orders: ", df_orders['days_since_prior_order'].isna().sum())
print()
print("Valores ausentes en df_orders:")
print(df_orders[df_orders['days_since_prior_order'].isna()])
```

    Número de Valores ausentes en df_orders:  28817
    
    Valores ausentes en df_orders:
            order_id  user_id  order_number  order_dow  order_hour_of_day  \
    28        133707   182261             1          3                 10   
    96        787445    25685             1          6                 18   
    100       294410   111449             1          0                 19   
    103      2869915   123958             1          4                 16   
    104      2521921    42286             1          3                 18   
    ...          ...      ...           ...        ...                ...   
    478880   2589657   205028             1          0                 16   
    478881   2222353   141211             1          2                 13   
    478907   2272807   204154             1          1                 15   
    478911   2499542    68810             1          4                 19   
    478930   1387033    22496             1          5                 14   
    
            days_since_prior_order  
    28                         NaN  
    96                         NaN  
    100                        NaN  
    103                        NaN  
    104                        NaN  
    ...                        ...  
    478880                     NaN  
    478881                     NaN  
    478907                     NaN  
    478911                     NaN  
    478930                     NaN  
    
    [28817 rows x 6 columns]
    


```python
# ¿Hay algún valor ausente que no sea el primer pedido del cliente?
        # No. Todos los valores ausentes corresponden a clientes haciendo su primer pedido.
print(df_orders[  (df_orders['days_since_prior_order'].isna()) & (df_orders['order_number'] ==1)   ])     #  Se obtiene el mismo número de registros 28817
print(df_orders[  (df_orders['days_since_prior_order'].isna()) & (df_orders['order_number'] > 1)   ])     #  Al buscar 'order_number' > 1 no hay registros de clientes con pedidos previos


```

            order_id  user_id  order_number  order_dow  order_hour_of_day  \
    28        133707   182261             1          3                 10   
    96        787445    25685             1          6                 18   
    100       294410   111449             1          0                 19   
    103      2869915   123958             1          4                 16   
    104      2521921    42286             1          3                 18   
    ...          ...      ...           ...        ...                ...   
    478880   2589657   205028             1          0                 16   
    478881   2222353   141211             1          2                 13   
    478907   2272807   204154             1          1                 15   
    478911   2499542    68810             1          4                 19   
    478930   1387033    22496             1          5                 14   
    
            days_since_prior_order  
    28                         NaN  
    96                         NaN  
    100                        NaN  
    103                        NaN  
    104                        NaN  
    ...                        ...  
    478880                     NaN  
    478881                     NaN  
    478907                     NaN  
    478911                     NaN  
    478930                     NaN  
    
    [28817 rows x 6 columns]
    Empty DataFrame
    Columns: [order_id, user_id, order_number, order_dow, order_hour_of_day, days_since_prior_order]
    Index: []
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

Desde el análisis inicial se había identificado que el campo 'days_since_prior_order' tenía valores nulos. Se estableció la hipótesis de que esos nulos se debían a que eran clientes haciendo su primer pedido. Se confirmo la hipótesis al confirmar que days_since_prior_order no tiene valores nulos en los registros de clientes con pedidos previos (es decir clientes que tienen order_number > 1)

<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>

Bien, es normal que esas filas tengan nulos, son los primeros pedidos de los clientes, y entonces no hay días desde un pedido anterior porque no existe ese pedido anterior.
    
</div>

### `order_products` data frame


```python
# Encuentra los valores ausentes
print("Número de Valores ausentes en df_order_products['add_to_cart_order']: ", df_order_products['add_to_cart_order'].isna().sum())
print("Número de Valores ausentes en df_order_products: ")
print(df_order_products.isna().sum())
```

    Número de Valores ausentes en df_order_products['add_to_cart_order']:  836
    Número de Valores ausentes en df_order_products: 
    order_id               0
    product_id             0
    add_to_cart_order    836
    reordered              0
    dtype: int64
    


```python
# ¿Cuáles son los valores mínimos y máximos en esta columna?
      # El mínimo es 1.00 y el máximo 64.00
print("El mínimo es 1.00 y el máximo 64.00 en la columna add_to_cart_order")
print(df_order_products.describe().round(2))     # Presenta el mín y máximo valor. Pero esta consulta no cuenta los valores NaN.
print()
print("Al usar value_counts sobre el campo 'add_to_cart_order' se confirman 836 valores NaN")
print( df_order_products['add_to_cart_order'].value_counts(dropna=False).head(50))   # Se confirma que hay 836 valores nulos.
```

    El mínimo es 1.00 y el máximo 64.00 en la columna add_to_cart_order
             order_id  product_id  add_to_cart_order   reordered
    count  4545007.00  4545007.00         4544171.00  4545007.00
    mean   1711165.93    25580.84               8.35        0.59
    std     985095.50    14095.52               7.08        0.49
    min          4.00        1.00               1.00        0.00
    25%     860817.00    13535.00               3.00        0.00
    50%    1711530.00    25272.00               6.00        1.00
    75%    2561311.00    37935.00              11.00        1.00
    max    3421079.00    49694.00              64.00        1.00
    
    Al usar value_counts sobre el campo 'add_to_cart_order' se confirman 836 valores NaN
    add_to_cart_order
    1.0     450046
    2.0     428199
    3.0     401907
    4.0     372861
    5.0     341807
    6.0     309884
    7.0     278186
    8.0     247364
    9.0     218825
    10.0    193083
    11.0    169835
    12.0    149429
    13.0    130890
    14.0    114393
    15.0     99921
    16.0     87225
    17.0     75760
    18.0     65758
    19.0     57032
    20.0     49420
    21.0     42649
    22.0     36911
    23.0     31815
    24.0     27393
    25.0     23555
    26.0     20173
    27.0     17361
    28.0     14903
    29.0     12766
    30.0     10867
    31.0      9249
    32.0      7921
    33.0      6750
    34.0      5747
    35.0      4902
    36.0      4216
    37.0      3629
    38.0      3121
    39.0      2646
    40.0      2269
    41.0      1934
    42.0      1655
    43.0      1418
    44.0      1220
    45.0      1045
    46.0       901
    NaN        836
    47.0       769
    48.0       666
    49.0       576
    Name: count, dtype: int64
    

Describe brevemente cuáles son tus hallazgos.

Al usar value_counts sobre el campo 'add_to_cart_order' se confirman 836 valores NaN. Es algo irregular. Debemos investigar mas el motivo y establecer cómo corregirlo.


```python
# Guarda todas las IDs de pedidos que tengan un valor ausente en 'add_to_cart_order'
ids_con_val_ausentes = df_order_products[df_order_products['add_to_cart_order'].isna()]['order_id']
print(ids_con_val_ausentes)
```

    737        2449164
    9926       1968313
    14394      2926893
    16418      1717990
    30114      1959075
                ...   
    4505662    1800005
    4511400    1633337
    4517562     404157
    4534112    1673227
    4535739    1832957
    Name: order_id, Length: 836, dtype: int64
    


```python
# ¿Todos los pedidos con valores ausentes tienen más de 64 productos?
       # si, se puede confirmar usando groupby y count
productos_max=df_order_products.groupby(by='order_id').count().sort_values(by='product_id',ascending=False)
print("La tabla nos da señales de que agrupando por order_id, hay un problema con el campo add_to_cart_order, pues deja de sumar unidades despues de 64 productos pedidos")
print(productos_max[60:80])
print()
print("El resultado vacío confirma que cuando orden_id agrupa mas de 64 productos pedidos, ya no se incrementa el recuento del campo add_to_cart_order por encima de 64")
print(productos_max[      (productos_max['add_to_cart_order'] > 64)  &   (productos_max['product_id'] > 64)  ])
print()
print("El resultado vacío confirma que cuando orden_id agrupa 64 o menos productos pedidos, siempre coincide el acumulado de orden_id y add_to_cart_order")
print(productos_max[      (productos_max['add_to_cart_order'] != productos_max['product_id'])  &   (productos_max['product_id'] <= 64)  ])



# Agrupa todos los pedidos con datos ausentes por su ID de pedido.
print()
pedidos_con_val_ausentes= df_order_products[df_order_products['add_to_cart_order'].isna()].groupby(by='order_id').count()
print("La tabla acumula en el campo 'product_id' el número de productos ordenados en exceso de 64 unidades, son estas unidades las que generan un valor nulo en el campo 'add_to_cart_order':")
print(pedidos_con_val_ausentes)


# Cuenta el número de 'product_id' en cada pedido y revisa el valor mínimo del conteo.
print()
print("Se aprecia que el máximo que alcanza a acumular el campo add_to_cart_order es 64")
print(pedidos_con_val_ausentes.describe())
print()
print("A fin de confirmar los datos se imprime el pedido 3383594 que sabemos que tuvo 64 + 5 productos solicitados")
print(df_order_products[df_order_products['order_id']==3383594].sort_values(by='add_to_cart_order'))

```

    La tabla nos da señales de que agrupando por order_id, hay un problema con el campo add_to_cart_order, pues deja de sumar unidades despues de 64 productos pedidos
              product_id  add_to_cart_order  reordered
    order_id                                          
    1529171           66                 64         66
    747668            65                 64         65
    9310              65                 64         65
    1598369           65                 64         65
    888470            65                 64         65
    1677118           65                 64         65
    2729254           65                 64         65
    2652650           65                 64         65
    2621907           65                 64         65
    2170451           65                 64         65
    3062914           64                 64         64
    1007609           64                 64         64
    1149773           64                 64         64
    989476            64                 64         64
    750981            64                 64         64
    3196508           63                 63         63
    1603091           63                 63         63
    2241712           63                 63         63
    88620             63                 63         63
    724656            63                 63         63
    
    El resultado vacío confirma que cuando orden_id agrupa mas de 64 productos pedidos, ya no se incrementa el recuento del campo add_to_cart_order por encima de 64
    Empty DataFrame
    Columns: [product_id, add_to_cart_order, reordered]
    Index: []
    
    El resultado vacío confirma que cuando orden_id agrupa 64 o menos productos pedidos, siempre coincide el acumulado de orden_id y add_to_cart_order
    Empty DataFrame
    Columns: [product_id, add_to_cart_order, reordered]
    Index: []
    
    La tabla acumula en el campo 'product_id' el número de productos ordenados en exceso de 64 unidades, son estas unidades las que generan un valor nulo en el campo 'add_to_cart_order':
              product_id  add_to_cart_order  reordered
    order_id                                          
    9310               1                  0          1
    61355             63                  0         63
    102236            31                  0         31
    129627             5                  0          5
    165801             6                  0          6
    ...              ...                ...        ...
    2999801            6                  0          6
    3125735           22                  0         22
    3308010           51                  0         51
    3347453            7                  0          7
    3383594            5                  0          5
    
    [70 rows x 3 columns]
    
    Se aprecia que el máximo que alcanza a acumular el campo add_to_cart_order es 64
           product_id  add_to_cart_order  reordered
    count   70.000000               70.0  70.000000
    mean    11.942857                0.0  11.942857
    std     12.898585                0.0  12.898585
    min      1.000000                0.0   1.000000
    25%      3.000000                0.0   3.000000
    50%      7.000000                0.0   7.000000
    75%     14.000000                0.0  14.000000
    max     63.000000                0.0  63.000000
    
    A fin de confirmar los datos se imprime el pedido 3383594 que sabemos que tuvo 64 + 5 productos solicitados
             order_id  product_id  add_to_cart_order  reordered
    2531790   3383594       19645                1.0          0
    1880235   3383594        2525                2.0          0
    1640026   3383594       25146                3.0          1
    2124274   3383594       48697                4.0          0
    2992211   3383594       27360                5.0          1
    ...           ...         ...                ...        ...
    1038616   3383594       15424                NaN          1
    1537276   3383594       15076                NaN          0
    2772397   3383594       46710                NaN          0
    3760909   3383594          63                NaN          0
    3784932   3383594       49144                NaN          1
    
    [69 rows x 4 columns]
    

<div class="alert alert-block alert-success">
<b>Comentario de Revisor        </b> <a class="tocSkip"></a>

Bien hecho, es probable que el sistema no permita más de 64 tipos de productos en esa columna, por lo que no guarda nada después
    
</div>

Describe brevemente cuáles son tus hallazgos.

El análisis demuestra que el campo add_to_cart_order no tiene valores correctos. Algo sucede cuando el número de productos incluídos en una orden excede de 64. Cuando se agrupa por 'order-id', a partir de 64 productos comprados el valor del campo add_to_cart_order se mantiene en 64. Por lo tanto no podríamos saber cual fue la secuencia de selección de productos del cliente despues del producto número 64.


```python
# Remplaza los valores ausentes en la columna 'add_to_cart? con 999 y convierte la columna al tipo entero.
df_order_products['add_to_cart_order']=df_order_products['add_to_cart_order'].fillna(999)
df_order_products['add_to_cart_order']=df_order_products['add_to_cart_order'].astype('int64')
df_order_products.info(show_counts=True)

```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 4545007 entries, 0 to 4545006
    Data columns (total 4 columns):
     #   Column             Non-Null Count    Dtype
    ---  ------             --------------    -----
     0   order_id           4545007 non-null  int64
     1   product_id         4545007 non-null  int64
     2   add_to_cart_order  4545007 non-null  int64
     3   reordered          4545007 non-null  int64
    dtypes: int64(4)
    memory usage: 138.7 MB
    

Describe brevemente tus hallazgos y lo que hiciste con ellos.

Idealmente, hubiera sido bueno conocer la secuencia que tienen los clientes en la selección de los productos dentro de una orden. Es algo que tenemos sólo hasta el producto elegido número 64 en la variable add_to_cart_order. Mas allá del 64 no lo sabemos porque en la información de origen este dato no se incluyó. En lugar de mantener el dato NaN en estos casos optamos por sustituírlo por 999, así que para cuaquier análisis que hagamos y que involucre a al campo add_to_cart_order lo debemos tomar en cuenta.

## Conclusiones

Escribe aquí tus conclusiones intermedias sobre el Paso 2. Preprocesamiento de los datos

Se identificaron situaciones de duplicidad y de datos incorrectos que fueron corregidos.

En el df_orders habían 15 pedidos duplicados que se eliminaron.

En el df_orders el campo 'days_since_prior_order' presentaba valores nulos. Se identificó que es un valor correcto, derivado de que se trata de pedidos que hace el cliente por primera vez.

En el df_products había nombres de producto incorrectos. Algunos se repetían exactamente, otros se repetían pasando todo a mayúsculas otros presentaban valores vacíos. No se hicieron ajustes. Se consideró que no afecta el análisis porque el campo llave es product_id, que no presenta duplicidad ni datos inválidos.

En los df_aisles y df_departments, se identificaron valores de tipo 'missing' que estaban generando un conflicto en el df_productos. Estos valores fueron sustituídos por 'Unknowon' para evitar el conflicto.

En el df_order_products, campo 'add_to_cart_order' que muestra la secuencia de productos seleccionados en una orden, se identificó que la secuencia llega a un máximo de 64. Después de ello ya no aumenta el contador y en su lugar se tenían datos nulos. Estos nulos se cambiaron por el valor 999. Con lo cual no debe afectarse nuestro análisis, pero sí tenerse presente.


# Paso 3. Análisis de los datos

Una vez los datos estén procesados y listos, haz el siguiente análisis:

# [A] Fácil (deben completarse todos para aprobar)

1. Verifica que los valores en las columnas `'order_hour_of_day'` y `'order_dow'` en la tabla orders sean razonables (es decir, `'order_hour_of_day'` oscile entre 0 y 23 y `'order_dow'` oscile entre 0 y 6).
2. Crea un gráfico que muestre el número de personas que hacen pedidos dependiendo de la hora del día.
3. Crea un gráfico que muestre qué día de la semana la gente hace sus compras.
4. Crea un gráfico que muestre el tiempo que la gente espera hasta hacer su siguiente pedido, y comenta sobre los valores mínimos y máximos.

### [A1] Verifica que los valores sean sensibles


```python
print(df_orders['order_hour_of_day'].min())
print(df_orders['order_hour_of_day'].max())
```

    0
    23
    


```python
print(df_orders['order_dow'].min())
print(df_orders['order_dow'].max())
```

    0
    6
    

Escribe aquí tus conclusiones

Los datos en la columna order_our_day tienen sentido pues establecen la hora del día y van de las 0 hrs a las 23 hrs.

Los datos en la columna order_dow tienen sentido pues establecen el día de la semana y van de 0 (domingo) a 6 (sábado).


<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>

Correcto, muy bien con el cálculo. Ahora estamos seguros que tenemos datos que hacen sentido en estas columnas.
    
</div>

### [A2] Para cada hora del día, ¿cuántas personas hacen órdenes?


```python
from matplotlib import pyplot as plt
df_orders['order_hour_of_day'].hist(bins=[0,0.99, 1.99, 2.99, 3.99, 4.99, 5.99, 6.99,7.99,8.99,9.99,10.99,11.99,12.99,13.99,14.99,15.99,16.99,17.99,18.99,19.99,20.99,21.99,22.99,23.99])
plt.title("Número de pedidos por hora")
plt.xlabel("hora del día (0hrs - 24hrs)")
plt.ylabel("suma de pedidos recibidos")
plt.xlim(0,25)
plt.show()
print()
print( df_orders['order_hour_of_day'].value_counts().sort_index())

```


    
![png](notebook_supermercado_files/notebook_supermercado_92_0.png)
    


    
    order_hour_of_day
    0      3180
    1      1763
    2       989
    3       770
    4       765
    5      1371
    6      4215
    7     13043
    8     25024
    9     35896
    10    40578
    11    40032
    12    38034
    13    39007
    14    39631
    15    39789
    16    38112
    17    31930
    18    25510
    19    19547
    20    14624
    21    11019
    22     8512
    23     5611
    Name: count, dtype: int64
    

Escribe aquí tus conclusiones

Llegan muy pocos pedidos en la madrugada. A partir de las 7am se incrementa el volúmen alcanzando un máximo entre las 10am y 11am.


<div class="alert alert-block alert-success">
<b>Comentario de Revisor        </b> <a class="tocSkip"></a>


Bien hecho con este gráfico, se observa que hay pedidos nocturnos y el peak llega a las 10 de la mañana

</div>

### [A3] ¿Qué día de la semana compran víveres las personas?


```python
df_orders['order_dow'].hist(bins=[0,0.99, 1.99, 2.99, 3.99, 4.99, 5.99, 6.99])
plt.title("Número de pedidos por día de la semana")
plt.xlabel("Día de la semana (0 domingo - 6 sábado)")
plt.ylabel("suma de pedidos recibidos")
plt.xlim(0,7)
plt.xticks([0.5,1.5,2.5,3.5,4.5,5.5,6.5],['domingo','lunes','martes','miércoles','jueves','viernes','sábado'])
plt.grid(False)
plt.show()
print()
print( df_orders['order_dow'].value_counts().sort_index())
```


    
![png](notebook_supermercado_files/notebook_supermercado_96_0.png)
    


    
    order_dow
    0    84090
    1    82185
    2    65833
    3    60897
    4    59810
    5    63488
    6    62649
    Name: count, dtype: int64
    

Escribe aquí tus conclusiones

Los días domingo y lunes son los de mayor volúmen para recibir pedidos, con volúmen arriba de 80,000. El resto de los días de la semana se mantiene con un volúmen cercano a 60,000.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor        </b> <a class="tocSkip"></a>

Correcto el gráfico, se aprecia que el domingo es el día con mayor cantidad de pedidos. Salvo el lunes, el resto de los días son muy parejos.



</div>

### [A4] ¿Cuánto tiempo esperan las personas hasta hacer otro pedido? Comenta sobre los valores mínimos y máximos.


```python


df_orders['days_since_prior_order'].hist(bins=[0,0.99, 1.99, 2.99, 3.99, 4.99, 5.99, 6.99,7.99,8.99,9.99,10.99,11.99,12.99,13.99,14.99,15.99,16.99,17.99,18.99,19.99,20.99,21.99,22.99,23.99,24.99,25.99,26.99,27.99,28.99,29.99,30.99])
plt.title("Días transcurridos desde el último pedido")
plt.xlabel("Día transcurridos)")
plt.ylabel("suma de pedidos recibidos")
plt.xlim(0,32)
plt.grid(False)
plt.show()
print()
print("Número mínimo observado de días transcurridos desde el último pedido:",df_orders['days_since_prior_order'].min())
print("Número máximo observado de días transcurridos desde el último pedido:",df_orders['days_since_prior_order'].max())
print()
print( df_orders['days_since_prior_order'].value_counts().sort_index())
```


    
![png](notebook_supermercado_files/notebook_supermercado_100_0.png)
    


    
    Número mínimo observado de días transcurridos desde el último pedido: 0.0
    Número máximo observado de días transcurridos desde el último pedido: 30.0
    
    days_since_prior_order
    0.0      9589
    1.0     20179
    2.0     27138
    3.0     30224
    4.0     31006
    5.0     30096
    6.0     33930
    7.0     44577
    8.0     25361
    9.0     16753
    10.0    13309
    11.0    11467
    12.0    10658
    13.0    11737
    14.0    13992
    15.0     9416
    16.0     6587
    17.0     5498
    18.0     4971
    19.0     4939
    20.0     5302
    21.0     6448
    22.0     4514
    23.0     3337
    24.0     3015
    25.0     2711
    26.0     2640
    27.0     2986
    28.0     3745
    29.0     2673
    30.0    51337
    Name: count, dtype: int64
    

Escribe aquí tus conclusiones

Hay un marcada tendencia a que pasen 7 días antes de realizarse el siguiente pedido. Esto tiene lógica por la gente que tiene una rutina de hacer su pedido en un día de la semana.

También hay una marcada tendencia a que pasen 30 días antes de realizarse el siguiente pedido. Esto puede explicarse de dos formas: (1) gente que tiene rutina de hacer su pedido mensualmente, o bien (2) que pasados mas de 30 días ya no se haga el recuento de días transcurridos correctamente dejando el valor default como 30.

Llama la atención que cerca de 10,000 clientes no esperaron ni un día para hacer su siguiente pedido.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor        </b> <a class="tocSkip"></a>

Muy bien, el gráfico es correcto. Nota que el tiempo de 30 días es muy alto, esto es sospechoso ya que es el valor más grande que se registra y es anormalmente grande respecto a los valores que vienen justo antes. Probablemente se debe a que el sistema no registra más de 30 días entre compras y todas las que son de más de ese tiempo las guarda como 30.
    
</div>

# [B] Intermedio (deben completarse todos para aprobar)

1. ¿Existe alguna diferencia entre las distribuciones `'order_hour_of_day'` de los miércoles y los sábados? Traza gráficos de barra de `'order_hour_of_day'` para ambos días en la misma figura y describe las diferencias que observes.
2. Grafica la distribución para el número de órdenes que hacen los clientes (es decir, cuántos clientes hicieron solo 1 pedido, cuántos hicieron 2, cuántos 3, y así sucesivamente...).
3. ¿Cuáles son los 20 principales productos que se piden con más frecuencia (muestra su identificación y nombre)?

### [B1] Diferencia entre miércoles y sábados para  `'order_hour_of_day'`. Traza gráficos de barra para los dos días y describe las diferencias que veas.


```python
df_miercoles=df_orders[df_orders['order_dow']==3]
df_miercoles_por_hora=df_miercoles.groupby(by='order_hour_of_day')['order_id'].count()
df_sabado=df_orders[df_orders['order_dow']==6]
df_sabado_por_hora=df_sabado.groupby(by='order_hour_of_day')['order_id'].count()
df_combinado = pd.DataFrame({
    'miercoles': df_miercoles_por_hora,
    'sabado': df_sabado_por_hora
})
df_combinado.plot(kind='bar', stacked=False)
plt.title("Número de pedidos por hora en miércoles y sábado")
plt.xlabel("hora del día (0hrs - 24hrs)")
plt.ylabel("suma de pedidos recibidos")
plt.xlim(-0.5,25)
plt.show()
print(df_combinado)

```


    
![png](notebook_supermercado_files/notebook_supermercado_105_0.png)
    


                       miercoles  sabado
    order_hour_of_day                   
    0                        373     464
    1                        215     254
    2                        106     177
    3                        101     125
    4                        108     118
    5                        170     161
    6                        643     451
    7                       1732    1619
    8                       3125    3246
    9                       4490    4311
    10                      5026    4919
    11                      5004    5116
    12                      4688    5132
    13                      4674    5323
    14                      4774    5375
    15                      5163    5188
    16                      4976    5029
    17                      4175    4295
    18                      3463    3338
    19                      2652    2610
    20                      1917    1847
    21                      1450    1473
    22                      1154    1185
    23                       718     893
    


```python
# También se puede obtener el mismo resultado usando histograma
df_miercoles['order_hour_of_day'].hist(bins=[0,0.99, 1.99, 2.99, 3.99, 4.99, 5.99, 6.99,7.99,8.99,9.99,10.99,11.99,12.99,13.99,14.99,15.99,16.99,17.99,18.99,19.99,20.99,21.99,22.99,23.99])
df_sabado['order_hour_of_day'].hist(alpha=0.6,bins=[0,0.99, 1.99, 2.99, 3.99, 4.99, 5.99, 6.99,7.99,8.99,9.99,10.99,11.99,12.99,13.99,14.99,15.99,16.99,17.99,18.99,19.99,20.99,21.99,22.99,23.99])
plt.title("Número de pedidos por hora en miércoles y sábado")
plt.xlabel("hora del día (0hrs - 24hrs)")
plt.ylabel("suma de pedidos recibidos")
plt.xlim(0,25)
plt.legend(['miércoles','sábado'])
plt.show()

```


    
![png](notebook_supermercado_files/notebook_supermercado_106_0.png)
    


Escribe aquí tus conclusiones

La distribución de pedidos recibidos en días miércoles y sábado son muy similares. En el caso de miércoles se observa un periodo de 3 horas de las 12:00 a las 15:00, donde hay una marcada baja de ordenes. Pudiera ser que en este periodo se presentó algún problema con el sistema, u otro motivo.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>

Bien hecho, muy bien al realizar el gráfico con las barras una junta a la otra, esto permite una comparación directa entre los días. El segundo gráfico también está bien, pero nota que se corre el peligro de que una barra tape a la otra.
    
</div>

### [B2] ¿Cuál es la distribución para el número de pedidos por cliente?


```python
#Grafica la distribución para el número de órdenes que hacen los clientes
#(es decir, cuántos clientes hicieron solo 1 pedido, cuántos hicieron 2, cuántos 3, y así sucesivamente...)
df_pedidos_por_cliente=df_orders.groupby(by='user_id')['order_id'].count()
df_pedidos_por_cliente.hist(figsize= [10,5],bins=[0.01, 1.01, 2.01, 3.01, 4.01, 5.01, 6.01,7.01,8.01,9.01,10.01,11.01,12.01,13.01,14.01,15.01,16.01,17.01,18.01,19.01,20.01,21.01,22.01,23.01,24.01,25.01,26.01,27.01,28.01,29.01,30.01])
plt.title("Número de pedidos por cliente")
plt.xlabel("Número de pedidos")
plt.ylabel("Número de clientes")
plt.xticks([0.5,1.5,2.5,3.5,4.5,5.5,6.5,7.5,8.5,9.5,10.5,11.5,12.5,13.5,14.5,15.5,16.5,17.5,18.5,19.5,20.5,21.5,22.5,23.5,24.5,25.5,26.5,27.5],[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28])
plt.xlim(0,28)
plt.grid(True)
#plt.figsize[10,5]
print("Número mínimo de pedidos por cliente:",df_pedidos_por_cliente.min())
print("Número máximo de pedidos por cliente:",df_pedidos_por_cliente.max())
plt.show()
print(df_pedidos_por_cliente.value_counts().sort_index())

```

    Número mínimo de pedidos por cliente: 1
    Número máximo de pedidos por cliente: 28
    


    
![png](notebook_supermercado_files/notebook_supermercado_110_1.png)
    


    order_id
    1     55357
    2     36508
    3     21547
    4     13498
    5      8777
    6      6012
    7      4240
    8      3019
    9      2152
    10     1645
    11     1308
    12      947
    13      703
    14      512
    15      437
    16      263
    17      184
    18      121
    19       85
    20       52
    21       22
    22       23
    23       19
    24        3
    25        1
    26        1
    28        1
    Name: count, dtype: int64
    

Escribe aquí tus conclusiones

La distribución es exponencial decreciente. Siendo 1 pedido por cliente el caso mas frecuente con 55,357. De ahí se reduce drásticamente la frecuencia a medida que aumenta el número de pedidos. El cliente con mayor número de pedidos es 28.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor          </b> <a class="tocSkip"></a>

Excelente, muy bien con el resultado. Observamos que la cantidad de pedidos de cliente es decreciente.



</div>

### [B3] ¿Cuáles son los 20 productos más populares (muestra su ID y nombre)?


```python
#primero vamos a usar merge() para crear un dataframe que incluya todos los datos que necesitamos.
df_order_products_details = df_order_products.merge(df_products, on='product_id', how='left')
print(df_order_products_details.head(10))
```

       order_id  product_id  add_to_cart_order  reordered  \
    0   2141543       11440                 17          0   
    1    567889        1560                  1          1   
    2   2261212       26683                  1          1   
    3    491251        8670                 35          1   
    4   2571142        1940                  5          1   
    5   2456893       21616                  4          1   
    6    644579       12341                  5          1   
    7   2231852       44925                 10          1   
    8   3185766       36259                 14          1   
    9    420019       23315                  4          1   
    
                                            product_name  aisle_id  department_id  
    0                     Chicken Breast Tenders Breaded       129              1  
    1                                         Bag Of Ice        37              1  
    2  Cafe Latte Pure Lightly Sweetened Iced Coffee ...        91             16  
    3                                     Diced Tomatoes        81             15  
    4                        Organic 2% Reduced Fat Milk        84             16  
    5                               Organic Baby Arugula       123              4  
    6                                      Hass Avocados        32              4  
    7                    Natural Provolone Cheese Slices        21             16  
    8                   Whole Natural Value Pack Almonds       117             19  
    9                           Organic Cold Brew Coffee        26              7  
    


```python

#luego vamos a agrupar los datos por product_id, contando cuantas veces se pide cada producto, y conservando el dato de product_name.
#adicionalmente conservaremos los datos aisle_id y department_id que podríamos necesitar mas adelante.

frecuencia_productos=df_order_products_details.groupby(by='product_id').agg({'product_name': 'first','aisle_id': 'first','department_id': 'first','product_id': 'count'})
print(frecuencia_productos.head(20))
```

                                                     product_name  aisle_id  \
    product_id                                                                
    1                                  Chocolate Sandwich Cookies        61   
    2                                            All-Seasons Salt       104   
    3                        Robust Golden Unsweetened Oolong Tea        94   
    4           Smart Ones Classic Favorites Mini Rigatoni Wit...        38   
    7                              Pure Coconut Water With Orange        98   
    8                           Cut Russet Potatoes Steam N' Mash       116   
    9                           Light Strawberry Blueberry Yogurt       120   
    10             Sparkling Orange Juice & Prickly Pear Beverage       115   
    11                                          Peach Mango Juice        31   
    12                                 Chocolate Fudge Layer Cake       119   
    13                                          Saline Nasal Mist        11   
    14                             Fresh Scent Dishwasher Cleaner        74   
    17                                          Rendered Duck Fat        35   
    18                        Pizza for One Suprema  Frozen Pizza        79   
    19           Gluten Free Quinoa Three Cheese & Mushroom Blend        63   
    21                           Small & Medium Dental Dog Treats        40   
    22                          Fresh Breath Oral Rinse Mild Mint        20   
    23                                     Organic Turkey Burgers        49   
    24          Tri-Vi-Sol® Vitamins A-C-and D Supplement Drop...        47   
    25                    Salted Caramel Lean Protein & Fiber Bar         3   
    
                department_id  product_id  
    product_id                             
    1                      19         280  
    2                      13          11  
    3                       7          42  
    4                       1          49  
    7                       7           2  
    8                       1          19  
    9                      16          21  
    10                      7         337  
    11                      7          16  
    12                      1          41  
    13                     11           2  
    14                     17           3  
    17                     12           1  
    18                      1          15  
    19                      9           1  
    21                      8           1  
    22                     11           6  
    23                     12         147  
    24                     11           2  
    25                     19         295  
    


```python
#finalmente ajustaremos los títulos de columnas para reconocerlos correctamente y mostraremos los 20 productos mas frecuentemente solicitados
frecuencia_productos.columns =   ['product_name','aisle_id','department_id','order_count']     # Se ajusta el nombre de la columna 'product_id'  a 'order_count'
print(frecuencia_productos.sort_values(by='order_count',ascending=False).head(20))

```

                            product_name  aisle_id  department_id  order_count
    product_id                                                                
    24852                         Banana        24              4        66050
    13176         Bag of Organic Bananas        24              4        53297
    21137           Organic Strawberries        24              4        37039
    21903           Organic Baby Spinach       123              4        33971
    47209           Organic Hass Avocado        24              4        29773
    47766                Organic Avocado        24              4        24689
    47626                    Large Lemon        24              4        21495
    16797                   Strawberries        24              4        20018
    26209                          Limes        24              4        19690
    27845             Organic Whole Milk        84             16        19600
    27966            Organic Raspberries       123              4        19197
    22935           Organic Yellow Onion        83              4        15898
    24964                 Organic Garlic        83              4        15292
    45007               Organic Zucchini        83              4        14584
    39275            Organic Blueberries       123              4        13879
    49683                 Cucumber Kirby        83              4        13675
    28204             Organic Fuji Apple        24              4        12544
    5876                   Organic Lemon        24              4        12232
    8277        Apple Honeycrisp Organic        24              4        11993
    40706         Organic Grape Tomatoes       123              4        11781
    

Escribe aquí tus conclusiones

Les gustan mucho las Bananas. Son los dos productos más solicitados. El campo order_count muestra el número de ordenes en las cuales se incluyó la compra de cada producto.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>

Correcto,  se ve que predominan los productos orgánicos y de uso diario
    
</div>

# [C] Difícil (deben completarse todos para aprobar)

1. ¿Cuántos artículos suelen comprar las personas en un pedido? ¿Cómo es la distribución?
2. ¿Cuáles son los 20 principales artículos que vuelven a pedirse con mayor frecuencia (muestra sus nombres e IDs de los productos)?
3. Para cada producto, ¿cuál es la tasa de repetición del pedido (número de repeticiones de pedido/total de pedidos?
4. Para cada cliente, ¿qué proporción de los productos que pidió ya los había pedido? Calcula la tasa de repetición de pedido para cada usuario en lugar de para cada producto.
5. ¿Cuáles son los 20 principales artículos que la gente pone primero en sus carritos (muestra las IDs de los productos, sus nombres, y el número de veces en que fueron el primer artículo en añadirse al carrito)?

### [C1] ¿Cuántos artículos compran normalmente las personas en un pedido? ¿Cómo es la distribución?


```python
# INSTRUCCIONES:
# ¿Cuántos artículos suelen comprar las personas en un pedido? ¿Cómo es la distribución?
#
#Tomando en cuenta que en la tabla df_order_products cada registro representa un artículo pedido, entonces el número de artículos
#que viene en cada pedido, se obtiene al contar cuantas veces se presenta el mismo valor order_id en este dataframe.
articulos_por_pedido=df_order_products.groupby(by='order_id').agg({'product_id': 'count'})
articulos_por_pedido.columns =   ['acum_articulos_por_pedido']
print(articulos_por_pedido.sort_values('acum_articulos_por_pedido',ascending=False).head())
```

              acum_articulos_por_pedido
    order_id                           
    61355                           127
    3308010                         115
    2136777                         108
    171934                          104
    1959075                          98
    


```python
#confirmamos que efectivamente el pedido 61355 tiene 127 artículos
print(df_order_products[df_order_products['order_id']==61355].sort_values(by='add_to_cart_order'))
```

             order_id  product_id  add_to_cart_order  reordered
    2897193     61355       47209                  1          0
    2652936     61355       34270                  2          1
    2187266     61355       41363                  3          0
    1628680     61355       28420                  4          1
    269840      61355       14233                  5          0
    ...           ...         ...                ...        ...
    3816634     61355       25659                999          0
    4063159     61355        1087                999          0
    4164200     61355        9484                999          0
    4126014     61355       12440                999          0
    4268097     61355       40709                999          0
    
    [127 rows x 4 columns]
    


```python
# trazamos la gráfica para obtener la distribución. usamos bins=127 porque sabemos que es el máximo de artículos en un pedido
articulos_por_pedido.hist(bins=127)
plt.title("Número de artículos por pedido")
plt.xlabel("Número de artículos")
plt.ylabel("Número de pedidos")
plt.xlim(0,127)
plt.show()

```


    
![png](notebook_supermercado_files/notebook_supermercado_123_0.png)
    



```python
#trazamos una nueva gráfica con zoom en el eje x en el rango 0 a 20 para entender mejor el comportamiento
articulos_por_pedido.hist(bins=127)
plt.title("Número de artículos por pedido")
plt.xlabel("Número de artículos")
plt.ylabel("Número de pedidos")
plt.xlim(0,20)
plt.show()
print(articulos_por_pedido.value_counts().sort_index().head(20))

```


    
![png](notebook_supermercado_files/notebook_supermercado_124_0.png)
    


    acum_articulos_por_pedido
    1                            21847
    2                            26292
    3                            29046
    4                            31054
    5                            31923
    6                            31698
    7                            30822
    8                            28539
    9                            25742
    10                           23248
    11                           20406
    12                           18539
    13                           16497
    14                           14472
    15                           12696
    16                           11465
    17                           10002
    18                            8726
    19                            7612
    20                            6771
    Name: count, dtype: int64
    

Escribe aquí tus conclusiones

El pedido mas grande fue de 127 artículos, sin embargo esto no es algo que se presente de forma común.

La gran mayoría de pedidos tienen menos de 20 artículos. El pedido mas común es de 5 artículos, con 31923 casos observados.

La distribución parece ser chi cuadrada.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor      </b> <a class="tocSkip"></a>

Muy bien con el gráfico, está correcto. Vemos que hay una distribución concentrada en los primeros valores, pero luego cae muy rápido
    
</div>

### [C2] ¿Cuáles son los 20 principales artículos que vuelven a pedirse con mayor frecuencia (muestra sus nombres e IDs de los productos)?


```python
# INSTRUCCIONES
# ¿Cuáles son los 20 principales artículos que vuelven a pedirse con mayor frecuencia (muestra sus nombres e IDs de los productos)?

# ANALISIS PRELIMINAR
# Para entender mejor los datos, primero vamos a resolver la pregunta:
#     ¿En df_order_products hay registros que coincidan en tener el mismo order_id y product_id?
# Creemos que esto podría ser posible. Si se repite "n" veces, el product_id 456, para el order_id 123,
#     ello sería indicativo de que se compraron "n" articulos del tipo 456, dentro del pedido 123

print("Registros duplicados en df_order_products tomando en cuenta los campos order_id y product_id : ", pd.concat([ col1,col2] ,axis='columns').duplicated().sum() )

#
# Se confirma que no hay 2 registros que tengan el mismo order_id y product_id. Por lo tanto se concluye que:
#  (1) dentro de cada order_id, sólamente se tiene 1 registro para cada product_id,
#  (2) en la información que nos dieron, sólo se menciona si un artículo se incluye en un pedido,
#      pero no nos dicen la cantidad comprada de dicho artículo, todo hace suponer en la información
#      que nos entregaron falta un campo donde nos digan la cantidad que se compró de cada producto en cada pedido.
#
# Nota aclaratoria:Dadas las circunstancias del caso, se realizará el análisis de 'frecuencia' solicitado, sin tomar en cuenta la
#                  cantidad que se solicitó de cada artículo en cada pedido. Se limitará el análilsis a revisar artículos pedidos
#                  vs no pedidos en cada pedido (sin importar cuantos artículos se solicitaron)
```

    Registros duplicados en df_order_products tomando en cuenta los campos order_id y product_id :  0
    


```python
# Necesitamos combinar frecuencia de productos adquiridos con sus nombres. Por lo tanto vamos a volver a usar la tabla que antes hicimos df_order_products_details
# Para obtener los que "vuelven a pedirse", debemos descartar en df_order_products_details los registros que se
# pidieron por vez primera. Es decir descartando los registros que tienen valor 'reordered'=0
df_order_products_reordered=df_order_products_details[df_order_products_details['reordered']==1]
print(df_order_products_reordered.head())
```

       order_id  product_id  add_to_cart_order  reordered  \
    1    567889        1560                  1          1   
    2   2261212       26683                  1          1   
    3    491251        8670                 35          1   
    4   2571142        1940                  5          1   
    5   2456893       21616                  4          1   
    
                                            product_name  aisle_id  department_id  
    1                                         Bag Of Ice        37              1  
    2  Cafe Latte Pure Lightly Sweetened Iced Coffee ...        91             16  
    3                                     Diced Tomatoes        81             15  
    4                        Organic 2% Reduced Fat Milk        84             16  
    5                               Organic Baby Arugula       123              4  
    


```python
# Ahora debemos agrupar por producto y obtener la frecuencia
# Usaremos .agg para aplicar distintos criterios de acumulación en los campos order_id, add_to_cart_order, etc.
frecuencia_productos_reordered=df_order_products_reordered.groupby(by='product_id').agg({'order_id': 'first','product_id': 'count','add_to_cart_order': 'last','reordered': 'first','product_name':'first', 'aisle_id':'first','department_id':'first'})
print("Tabla de todos los productos que se han comprado, con el número de órdenes en que se han inclído")
print(frecuencia_productos_reordered.head())
# Ahora debemos ajustar los nombres de las columnas para reconocer las que son valores agrupados o valores de ejemplo.
frecuencia_productos_reordered.columns =   ['order_id_example','order_count_solo_reorder','add_to_cart_order_last','reordered_first', 'product_name','aisle_id','department_id']     # Se ajusta el nombre de la columna 'product_id'  a 'acum_product'
print()
print("Tabla de todos los productos que se han comprado, con el número de órdenes en que se han inclído, ajustando nombre de columnas")
print(frecuencia_productos_reordered.head())
```

    Tabla de todos los productos que se han comprado, con el número de órdenes en que se han inclído
                order_id  product_id  add_to_cart_order  reordered  \
    product_id                                                       
    1            1104373         158                  8          1   
    3            2851416          31                  7          1   
    4            1339754          25                  7          1   
    7            1806224           1                  6          1   
    8            2292930           6                  4          1   
    
                                                     product_name  aisle_id  \
    product_id                                                                
    1                                  Chocolate Sandwich Cookies        61   
    3                        Robust Golden Unsweetened Oolong Tea        94   
    4           Smart Ones Classic Favorites Mini Rigatoni Wit...        38   
    7                              Pure Coconut Water With Orange        98   
    8                           Cut Russet Potatoes Steam N' Mash       116   
    
                department_id  
    product_id                 
    1                      19  
    3                       7  
    4                       1  
    7                       7  
    8                       1  
    
    Tabla de todos los productos que se han comprado, con el número de órdenes en que se han inclído, ajustando nombre de columnas
                order_id_example  order_count_solo_reorder  \
    product_id                                               
    1                    1104373                       158   
    3                    2851416                        31   
    4                    1339754                        25   
    7                    1806224                         1   
    8                    2292930                         6   
    
                add_to_cart_order_last  reordered_first  \
    product_id                                            
    1                                8                1   
    3                                7                1   
    4                                7                1   
    7                                6                1   
    8                                4                1   
    
                                                     product_name  aisle_id  \
    product_id                                                                
    1                                  Chocolate Sandwich Cookies        61   
    3                        Robust Golden Unsweetened Oolong Tea        94   
    4           Smart Ones Classic Favorites Mini Rigatoni Wit...        38   
    7                              Pure Coconut Water With Orange        98   
    8                           Cut Russet Potatoes Steam N' Mash       116   
    
                department_id  
    product_id                 
    1                      19  
    3                       7  
    4                       1  
    7                       7  
    8                       1  
    


```python
# Ahora debemos mostrar el resultado de los 20 artículoos  mas solicitado con sus nombres y número de artículo
print(frecuencia_productos_reordered[['product_name','order_count_solo_reorder']].sort_values(by='order_count_solo_reorder',ascending=False).head(20))
```

                            product_name  order_count_solo_reorder
    product_id                                                    
    24852                         Banana                     55763
    13176         Bag of Organic Bananas                     44450
    21137           Organic Strawberries                     28639
    21903           Organic Baby Spinach                     26233
    47209           Organic Hass Avocado                     23629
    47766                Organic Avocado                     18743
    27845             Organic Whole Milk                     16251
    47626                    Large Lemon                     15044
    27966            Organic Raspberries                     14748
    16797                   Strawberries                     13945
    26209                          Limes                     13327
    22935           Organic Yellow Onion                     11145
    24964                 Organic Garlic                     10411
    45007               Organic Zucchini                     10076
    49683                 Cucumber Kirby                      9538
    28204             Organic Fuji Apple                      8989
    8277        Apple Honeycrisp Organic                      8836
    39275            Organic Blueberries                      8799
    5876                   Organic Lemon                      8412
    49235            Organic Half & Half                      8389
    

Escribe aquí tus conclusiones

Despues de haber descartado los pedidos de artículos que los usuarios compran por vez primera, las bananas siguen siendo los artículos que se incluyen con mayor regularidad dentro de una orden.

Esto no siginifica que sea el artículo mas vendido. Esa conclusión sólo la podríamos dar si se hubiera incluído dentro de la información el dato de unidades_vendidas.

Llama la atención que el top 20 de artículos que vuelven a pedirse con mayor frecuencia incluye sólo productos frescos y la mayor parte de ellos del tipo orgánico. Esta claro que nuestra calidad y catálogo de productos es una ventaja competitiva.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>

Bien hecho, vemos que los productos más pedidos también son los más reordenados
    
</div>

### [C3] Para cada producto, ¿cuál es la proporción de las veces que se pide y que se vuelve a pedir?

<div class="alert alert-block alert-danger">
<b>Comentario de Revisor          </b> <a class="tocSkip"></a>


Interesante forma de análisis. Estás viendo que tanto se repite cada producto, dentro de los pedidos y las reordenes. Aunque esto es bueno para observar qué tan prevalentes son los productos a lo largo de todos los pedidos, en esta parte se requiere un porcentaje de reorden de un total de pedidos de cada producto. Para esta parte debes contar la cantidad de veces que fueron reordenados y dividirlo por la cantidad de veces que fueron ordenados. Esto también se puede obtener cuando se calcula la media de la variable reordered:
    
    (
        df_order_products
        .merge(df_products, on='product_id')
        .groupby(['product_id', 'product_name'])['reordered'].mean()
        .sort_values(ascending=False).reset_index()
    )
    




</div>


```python
# tomando en cuenta los comentarios recibidos ajusto mi respuesta en seguida.
#Calculamos la proporción de reorden de cada producto, agrupando los productos y sacando el promedio del campo 'reordered'
prop_prods_reordenados=df_order_products.groupby(by='product_id').agg({'reordered': 'mean'}).reset_index()
# Agreguemos nombres de productos a la tabla
prop_prods_reordenados = prop_prods_reordenados.merge(df_products, on='product_id', how='left')
# Conservamos las columnas relevantes. Borramos las columnas aisle_id y department_id
prop_prods_reordenados = prop_prods_reordenados[['product_id','product_name','reordered']]
# Ordenamos la tabla para mostrar los productos que tienen mayor proporción de reorden
prop_prods_reordenados = prop_prods_reordenados.sort_values(by='reordered',ascending=False)
# Ajustamos nombres de columnas para que resulten mas claras
prop_prods_reordenados.columns =   ['product_id','product_name','proporcion_productos_reordenados']

```


```python
# Presentamos la tabla de resultados
print("Ejemplos al azar de productos")
print(prop_prods_reordenados.sample(10))
print()
print("Productos con mayor proporción de reorden")
print(prop_prods_reordenados.head(10))

# Graficamos para poder interpretar los resultados
# Antes, ordenamos los datos
prop_prods_reordenados = prop_prods_reordenados.sort_values(by='proporcion_productos_reordenados').reset_index()
print(prop_prods_reordenados)

prop_prods_reordenados['proporcion_productos_reordenados'].plot()
plt.title("Proporción productos reordenados vs productos totales comprados")
plt.xlabel("Producto")
plt.ylabel("Proporción de reorden en porductos")
plt.grid(True)
plt.show()

```

           index  product_id                                       product_name  \
    0        266         295      Chewables Extra Strength Cherry Creme - 48 CT   
    1      45018       49088  Natural Body & Foot Powder with Tea Tree Oil &...   
    2      45019       49089                          Gluten-Free Soup Crackers   
    3        289         324  Strong & Kind Roasted Jalapeno Almond Protein Bar   
    4      31900       34787                        Flat Anchovies in Olive Oil   
    ...      ...         ...                                                ...   
    45568  32861       35834                           Bulgarian Organic Yogurt   
    45569  32921       35897                                           Ham Hock   
    45570  40998       44743                                     Real Zero Cola   
    45571  41002       44747                        Frozen Organic Blackberries   
    45572   7463        8189              2% Milk Reduced Fat Colby Jack Cheese   
    
           proporcion_productos_reordenados  
    0                                   0.0  
    1                                   0.0  
    2                                   0.0  
    3                                   0.0  
    4                                   0.0  
    ...                                 ...  
    45568                               1.0  
    45569                               1.0  
    45570                               1.0  
    45571                               1.0  
    45572                               1.0  
    
    [45573 rows x 4 columns]
    


    
![png](notebook_supermercado_files/notebook_supermercado_137_1.png)
    



```python

```

Escribe aquí tus conclusiones

Vemos muchos productos que se reordenan al 0% lo cual puede significar que no se han pedido en lo absoluto o bien que sólo se pidieron por primera vez (reordered=0). Tendríamos que investigar mas profundamente el tema para analizar la conveniencia de impulsar la venta de productos que el cliente no conoce o que tal vez tengan un precio no competitivo.
También vemos otros productos tienen por resultado 100%, estos productos son los mas ganadores sin duda.   
También vemos otros productos que se piden en algún porcentaje de recompra mayor a 0% y menor a 100%

### [C4] Para cada cliente, ¿qué proporción de sus productos ya los había pedido?

<div class="alert alert-block alert-danger">
<b>Comentario de Revisor          </b> <a class="tocSkip"></a>

En esta parte, según lo que entiendo de tu procedimiento, mencionas que los productos reordenados son cuando `reordered=0`, sin embargo, esto no es así, es cuando esa columna vale 1 que se indica que el producto se reordeno. Para esta parte se requiere un desarrollo similar a la parte anterior, contar las veces que un producto fue reordenado, vs las veces que no. Para ello, basta con utilizar `mean()` de reordered al agrupar por usuario, ya que eso equivale a suma(reordered)/conteo(reordered)=(cantidad veces que reordered fue 1)/(cantidad de veces que se pidió algo).

Como el usuario no está en `df_order_products`, deberás hacer un merge para incluir esa variable y poder agrupar por usuario.



</div>


```python
# tomando en cuenta los comentarios recibidos ajusto mi respuesta en seguida.
# usamos merge para agregar el user_id a la tabla de order_products. El campo llave es order_id
order_products_por_usuario= df_order_products.merge(df_orders, on='order_id', how='left')

# Ahora conservamos las columnas que necesitamos: product_id, user_id, reordered
order_products_por_usuario = order_products_por_usuario[['user_id','product_id','reordered']]
# Ahora agrupamos por cliente y obtenemos el promedio en el campo reordered
order_products_por_usuario = order_products_por_usuario.groupby(by='user_id').agg({'reordered': 'mean'}).reset_index()
# Ajustamos nombres de columnas para que resulten mas claras
order_products_por_usuario.columns =   ['user_id','proporcion_productos_reordenados']


```


```python
# Presentamos la tabla con resultados
print()
print(order_products_por_usuario.head(20))


# Graficamos para poder interpretar los resultados
# Antes, ordenamos los datos
order_products_por_usuario = order_products_por_usuario.sort_values(by='proporcion_productos_reordenados').reset_index()
print(order_products_por_usuario)

order_products_por_usuario['proporcion_productos_reordenados'].plot()
plt.title("Proporción de productos reordenados por cliente")
plt.xlabel("Clientes")
plt.ylabel("Proporción de productos reordenados")
plt.grid(True)
plt.show()



```

             index  user_id  proporcion_productos_reordenados
    0       115784   159721                               0.0
    1        95704   132132                               0.0
    2        11838    16243                               0.0
    3        11833    16237                               0.0
    4       139684   192599                               0.0
    ...        ...      ...                               ...
    149621   60057    82878                               1.0
    149622   15996    22030                               1.0
    149623   15998    22032                               1.0
    149624      65       88                               1.0
    149625   81358   112123                               1.0
    
    [149626 rows x 3 columns]
    


    
![png](notebook_supermercado_files/notebook_supermercado_143_1.png)
    


Escribe aquí tus conclusiones

Vemos un número imortante de clientes con proporción 0% (poco menos de 20,000 clientes). Esto debe investigarse con mas detalle. Pueden ser usuarios nuevos que apenas compraron sus primeros productos y por eso tienen todos sus productos con reordered=0, o bien puede tratarse de clientes que ya no compraron ningún producto y deberíamos intentar reconquistar y revisar si hubo algún problema.

Para el resto de los clientes, vemos que se distribuyen uniformemente los porcentajes de reorden.

### [C5] ¿Cuáles son los 20 principales artículos que las personas ponen primero en sus carritos?


```python
# INSTRUCCIONES
# ¿Cuáles son los 20 principales artículos que la gente pone primero en sus carritos
# (muestra las IDs de los productos, sus nombres, y el número de veces en que fueron el primer artículo en añadirse al carrito)?

# MI ANALISIS DE COMO RESOLVERLO
# Primero tendríamos que aislar los productos que los clientes pusieron primero en sus carros usando la df_order_products, con 'add_to_cart_order'= 1.
# Luego tendríamos que hacer el recuento de product_id para identificar los que tienen mayor frecuencia, y ordenarlos de mayor a menor frecuencia.

```


```python
 # aislar los productos que los clientes pusieron primero en sus carros usando la df_order_products, con 'add_to_cart_order'= 1.
 df_solicitados_primero = df_order_products[df_order_products['add_to_cart_order']==1].reset_index()
 print(df_solicitados_primero.head(5))
 print()
 print(type(df_solicitados_primero))

#

```

       index  order_id  product_id  add_to_cart_order  reordered
    0      1    567889        1560                  1          1
    1      2   2261212       26683                  1          1
    2     14   1961225       37553                  1          1
    3     16    639939       10017                  1          1
    4     23    750040        8518                  1          0
    
    <class 'pandas.core.frame.DataFrame'>
    


```python
# Hacer el recuento de product_id para identificar los que tienen mayor frecuencia, y ordenarlos de mayor a menor frecuencia.
df_sol_prim = df_solicitados_primero.groupby(by='product_id').agg({'add_to_cart_order': 'count'})
df_sol_prim.columns =   ['primer_producto_en_el_cart_frecuencia']
df_sol_prim.sort_values(by='primer_producto_en_el_cart_frecuencia',ascending=False,inplace=True)
df_sol_prim=df_sol_prim.merge(df_products, on='product_id', how='left')
print(df_sol_prim.head(20))
```

        product_id  primer_producto_en_el_cart_frecuencia  \
    0        24852                                  15562   
    1        13176                                  11026   
    2        27845                                   4363   
    3        21137                                   3946   
    4        47209                                   3390   
    5        21903                                   3336   
    6        47766                                   3044   
    7        19660                                   2336   
    8        16797                                   2308   
    9        27966                                   2024   
    10       44632                                   1914   
    11       49235                                   1797   
    12       47626                                   1737   
    13         196                                   1733   
    14       38689                                   1397   
    15       26209                                   1370   
    16       12341                                   1340   
    17        5785                                   1310   
    18       27086                                   1309   
    19       22935                                   1246   
    
                       product_name  aisle_id  department_id  
    0                        Banana        24              4  
    1        Bag of Organic Bananas        24              4  
    2            Organic Whole Milk        84             16  
    3          Organic Strawberries        24              4  
    4          Organic Hass Avocado        24              4  
    5          Organic Baby Spinach       123              4  
    6               Organic Avocado        24              4  
    7                  Spring Water       115              7  
    8                  Strawberries        24              4  
    9           Organic Raspberries       123              4  
    10   Sparkling Water Grapefruit       115              7  
    11          Organic Half & Half        53             16  
    12                  Large Lemon        24              4  
    13                         Soda        77              7  
    14     Organic Reduced Fat Milk        84             16  
    15                        Limes        24              4  
    16                Hass Avocados        32              4  
    17  Organic Reduced Fat 2% Milk        84             16  
    18                  Half & Half        53             16  
    19         Organic Yellow Onion        83              4  
    

Escribe aquí tus conclusiones

Las Bananas son el artículo que primero ponen los clientes en sus carritos de compra. En 15,562 ordenes fué el primer producto que se puso en el carrito. Coincide que las Bananas también son el artículo mas comprado.

La lista de los artículos que mas frecuentemente se ponen al principio en el carrito de compras es de productos frescos y en su mayoría orgánicos.

<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>

Buen trabajo. Al parecer los productos orgánicos representan el grueso de ventas en varios aspectos
    
</div>

### Conclusion general del proyecto:

Los pedidos estan concentrados entre las 9 y las 16hrs. Pudiera ser un cuello de botella a analizar. De confirmarse que se trata de un cuello de botella podría analizarse la opción de dar descuento en horarios fuera de este periodo.

No hay mucha diferencia en el número de pedidos que se tienen en los distintos días de la semana.

Llama la atención que cerca de 10,000 clientes no esperaron ni un día para hacer su siguiente pedido. Todo indica que una vez realizado su pedido, se dieron cuenta que se les había ovidado solicitar algún artículo. Sería bueno analizar estos casos con mas detenimiento para que el mensajero sólo haga un viaje a la casa del cliente (consolidar los pedidos)

Llama la atención que el número máximo de diás transcurridos desde el último pedido sea 30 días. Todo indica que pasado este tiempo ya no se incrementa el contador. Esto es un tema analizar, puesto que esta variable es muy relevante para identificar clientes que deajaron de usar el servicio, y debería detonar llamadas o promociones para averiguar el motivo y recuperar a clientes.

El pedido mas común es de 5 artículos, con 31923 casos observados. Esto nos habla de que muchos clientes usan el servicio para pedir productos ocasionales, suponemos que el "super grande" lo hacen físicamente en una tienda.

Llama la atención que el top 20 de artículos que vuelven a pedirse con mayor frecuencia incluye sólo productos frescos y la mayor parte de ellos del tipo orgánico. Este tipo de artículos también son los primeros que el cliente pone en su carrito de compras. Esta claro que nuestra calidad y catálogo de productos frescos (en particular orgánicos) es una ventaja competitiva.


Fue un reto interesante utilizar las herramientas que aprendimos de Pandas para este proyecto.



<div class="alert alert-block alert-success">
<b>Comentario de Revisor            </b> <a class="tocSkip"></a>


Excelentes conclusiones. Haces muy bien al incluir valores de las métricas muy importantes y resumes los principales hallazgos, buen trabajo!
    
</div>
