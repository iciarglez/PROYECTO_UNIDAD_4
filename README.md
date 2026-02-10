# Certificación energética de edificios: análisis de la Comunidad Valenciana

## Descripción del proyecto

La Directiva (UE) 2024/1275 del Parlamento Europeo y del Consejo, de 24 de abril de 2024, relativa a la eficiencia energética de los edificios, fue publicada el 8 de mayo de 2024 en el Diario oficial de la Unión Europea.
La nueva Directiva de Eficiencia Energética en Edificios buscar acelerar el ritmo de renovación de edificios en la UE, especialmente aquellos con peor comportamiento energético, ya que se constituye como un pilar clave para garantizar los objetivos de descarbonización. https://www.miteco.gob.es/es/energia/eficiencia/epbd2024.html

Este proyecto analiza el parque de viviendas de la Comunidad Valenciana, poniendo el foco en los certificados de eficiencia energética de los edificios.
El objetivo es estudiar la evolución de las certificaciones de eficiencia energética a lo largo de la última década, en las tres provincias de la Comunidad Valenciana.v



## Fuentes de datos

La fuente de datos principal es datos.gob.es: https://datos.gob.es/es/catalogo/a10002983-certificacion-energetica-de-edificios

En esta URL se encuentran los tres ficheros csv con los datos de cada provincia: Castellón, Valencia y Alicante. En la ficha técnica se encuentra la dirección de los datos de la Generalitat Valenciana (https://dadesobertes.gva.es/dataset/med-eficiencia-energetica-edificis-ivace), así como la fecha de última actualización, 09/04/2024.

Para poder profundizar en la materia, se dispone de un enlace a la página del IVACE, el Instituto Valenciano de la Competitividad Empresarial: https://gceedadesobertes.aven.es/dadesobertes. Desde esta página se accede a un documento que presenta la descripción de los datos de la descarga, adjunto en la carpeta del proyecto. 

Los datos de 2024 (último procesamiento 16/05/2025) se encuentran en la siguiente dirección: https://gceedadesobertes.aven.es/dadesobertes/Analisi?anyo=2024


## Pasos del proyecto

### Detalle de cada archivo

En este proyecto están disponibles los siguientes documentos:

- Readme_PROYECTO_U4 # Descripción del proyecto
- certificacion-energetica-de-edificios-en-alicante.csv  # Fichero csv original con los datos de Alicante hasta 2023
- certificacion-energetica-de-edificios-en-castellon.csv  # Fichero csv original con los datos de Castellón hasta 2023
- certificacion-energetica-de-edificios-en-valencia.csv  # Fichero csv original con los datos de Valencia hasta 2023
- etiquetas_2024_ALICANTE.csv # Fichero csv original con los datos de 2024 de Alicante
- etiquetas_2024_CASTELLÓN.csv # Fichero csv original con los datos de 2024 de Castellón
- etiquetas_2024_VALENCIA.csv # Fichero csv original con los datos de 2024 de Valencia
- Descripcion_datos_de_descarga.pdf # Descripción de los datos de la descarga (fuente IVACE)
- CEE_Comunidad_Valenciana_data0.xlsx # Documento excel donde están cargados todos los datos de los csv, fichero creado con los registros originales, en la hoja "data0". 
- CEE_Comunidad_Valenciana.xlsm # Documento excel donde están la tabla de datos en valores, las tablas dinámicas y el dashbarod, donde se desarrolla el proyecto


Se ha decidido trabajar en el excel CEE_Comunidad_Valenciana.xlsm con todos los datos en valores y que no sea un fichero tan pesado. A la hora de trabajar, debido al volumen de filas, era muy complicado aplicar filtros, realizar los análisis, crear tablas dinámicas y gráficos dinámicos. Se mantienen los datos originales y las fórmulas de las nuevas columnas creadas en el excel CEE_Comunidad_Valenciana_data0.xlsx, hoja data0. No se adjunta por motivos prácticos, dado que consiste en la carga de todos los csv ya compartidos.



### Análisis

#### 1. Carga de datos

- Se importan los datos de los 3 csv en el excel CEE_Comunidad_Valenciana_data0.xlsx, en la hoja "data0".
- Se importan los datos de los 3 csv con las etiquetas de 2024, en el excel CEE_Comunidad_Valencia.xlsx_data0, en la hoja "data0".
- Se copian estos datos en una nueva hoja, "data", del excel CEE_Comunidad_Valenciana.xlsm, que es donde se harán las modificaciones oportunas, los datos con los que se creará el dashboard.
- No descargamos los datos de 2025 porque no está el año completo, y para este informe se decide hacer el análisis con años cerrados.



#### 2. Identificación de variables

- Junto con el documento "Descripcion_datos_de_descarga.pdf", se identifican las columnas de la tabla, cada una de las variables del proyecto.
- Es importante destacar que no existe una variable que haga referencia directa al año de la certificación. Se obtendrá en la fase de Transformación y limpieza de datos.
- El año que aparece en la variable "REGISTRO" es el año de inicio del registro.
- La fecha máxima de validez del CEE es de 10 años, contados desde la fecha de validación de la calificación energética por parte del técnico certificador, excepto cuando la calificación energética es G, que será de 5 años. Aquellos certificados registrados antes de la entrada en vigor del RD 390/2021 y que tuvieran una calificación energética G, la validez máxima es de 10 años.
- En cuanto a la referencia catastral, la variable REFERENCIA_CATASTRAL contiene las referencia/s catastral/es separadas por una coma, según información disponible en la Sede Electrónica del Catastro. En edificios de Nueva Construcción la referencia catastral no está disponible para registros anteriores a 2016.

El fichero inicial cuenta con 890.220 filas y 14 columnas.



#### 3. Limpieza y transformación de los datos

Para llevar a cabo la limpieza y transformación de los datos, se tiene en cuenta la información del documento "Descripcion_datos_de_descarga.pdf", que contiene los metadatos de los datos del proyeto.

- Se crea la variable "AÑO_INICIO", a partir del año que se encuentra en la variable "REGISTRO".
- Se crea la variable "AÑO_FIN", a partir del año de la variable "VALIDO_HASTA". Se le resta 10, dado que la validez máxima es de 10 años.
- Se crea la variable "MES_FIN", a partir del mes de la variable "VALIDO_HASTA".
- Se crea la variable "AÑO_CEE". El año corresponde al año de finalización y presentación ante la Administración del registro del CEE (pago de la tasa). Esta es una de las variables principales para el análisis.
    - Si la certificación no es G, el valor de esta variable es igual a "AÑO_FIN".
    - Si la certificación de consumo o emisiones es G, y AÑO_FIN es menor que 2021, el valor de esta variable es igual a "AÑO_FIN".
    - Si la certificación de consumo o emisiones es G, y AÑO_FIN es mayor que 2021, el valor de esta variable es igual a "AÑO_FIN" - 5.
    - Si la certificación de consumo o emisiones es G, y AÑO_FIN es igual a 2021, evaluamos el valor de la variable "MES_FIN". Si es menos que 6 (anterior a junio), el valor de esta variable es igual a "AÑO_FIN" Sin embargo, si es posterior a junio, el valor será igual a "AÑO_FIN" - 5.

La lógica descrita queda recogida en la fórmula:
=SI(O([@[CONSUMO_EP_LETRA]]<>"G";[@[EMISIONES_CO2_LETRA]]<>"G");[@[AÑO_FIN]];SI.CONJUNTO(O([@[AÑO_FIN]]<2021;Y([@[AÑO_FIN]]=2021;[@[MES_FIN]]<6));[@[AÑO_FIN]];O([@[AÑO_FIN]]>2021;Y([@[AÑO_FIN]]=2021;[@[MES_FIN]]>=6));[@[AÑO_FIN]]-5))


No hay casos donde se de una certificación G a partir de junio de 2021 (fecha en la que entra en vigor el RD 390/2021), pero la fórmula está preparada por si hubiera algún caso.


- Se crea la variable "NORMATIVA", que tiene en cuenta el año de construcción del edificio. Las normas de construcción que se tienen en cuenta son (https://www.codigotecnico.org/QueEsCTE/Historia.html): 
    1. Anterior a MV (<1957): Construcciones previas a la primera normativa moderna (MV).
    2. Normas MV (1957-1977): Normas M.V. (Mínimas de la Vivienda), regulación técnica básica previa a las NBE. 
    3. Normas NBE (1977-2006): Normas Básicas de la Edificación (NBE), como NBE-CT-79, NBE-AE-88, etc. 
    4. CTE (>2006): Código Técnico de la Edificación, vigente desde 2006 en adelante.


- Se crea la variable "ESTADO", que indica si el certificado está caducado o está vigente, atendiendo a la variable "VALIDEZ_HASTA"

Datos faltantes o outliers:

- Se comprueba que no hay duplicados ni datos faltantes en la columna "REGISTRO", que es la variable que determina la unicidad de cada registro.
- Si algún registro no tiene certificación en consumo o en emisiones, no se tendrá en cuenta en el análisis, ya que será una de las variables principales del análisis y no la podemos imputar. Hay 544, de 890.220, por lo que no alterará las conclusiones del análisis. Para ello, no se incluirá la categoría "Vacío" en la tabla dinámica cuando se tabulen los datos.
- Se crea la variable "AÑO_CONS", en la que, para años inferiores a 1700 o superiores a 2024, se deja vacío. Al igual que con los vacíos en las certificaciones de consumo o emisiones, se excluirá del análisis, al no añadirse en las tablas dinámicas. Hay 100 registros vacíos.
- Hay registros que no tienen referencia catastral, 95. Todos son de los años 2014, 2015 y 2016. No es preocupante, es posible continuar el análisis asumiendo estos datos faltantes. Además, hay códigos de catastro duplicados, pero no es extraño ya que un inmueble puede obtener certificaciones de eficiencia energética en distintos momentos (por ejemplo, en nueva construcción y después de una reforma, o en el momento de venta y después de una reforma). Parte del análisis de este proyecto es estudiar la evolución de las certificaciones en el tiempo, por lo que no podemos eliminar estos valores duplicados (119.854 registros).
 
Dado el volumen de códigos de referencia catastral duplicados, se analizan en otra hoja, "duplicados". Hay 119.854 registros con una referencia catastral que aparece más de una vez (no se tienen en cuenta los que no presentan referencia catastral). Se eliminan los que están duplicados, el registro completo, sin tener en cuenta el código de registro (que es único). Hay 5 casos. El resto de casos presenta alguna diferencia, por lo que los mantenemos. Procedemos a eliminar los duplicados, sin tener en cuenta la variable "REGISTRO", en el conjunto completo de datos. En total hay 890.215 valores únicos.



### Dashboard

Se crea un dashboard con la siguiente información:
- Número de registros de certificaciones de eficiencia energética para el periodo 2014-2024.
- Porcentaje de certificaciones vigentes.
- Porcentaje de certificaciones de consumo con letra A del total de certificaciones registradas, entre las vigentes.
- Porcentaje de certificaciones de emisiones con letra A del total de certificaciones registradas, entre las vigentes.

A continuación, se presentan los gráficos que se ven afectados tanto por la segmentación como por el tipo de certificación (consumo o emisiones):
- Gráfico de líneas que muestra la evolución de los certificados de eficiencia energética, en porcentaje, según la letra. Permite filtrar por letra, en el propio gráfico.
- Gráficos circulares, que muestra ela distribución de certificaciones de tipo A, G y totales, por provincias.
- Gráfico de barras horizontales que muestra las certificaciones A y G por tipo de uso del inmueble.
- Gráfico de barras verticales en el que se aprecia el número de certificaciones de las viviendas (vivienda unifamiliar, vivienda individual y edificio de viviendas), por tipo de letra.

Para cambiar entre consumo y emisiones, se dispone de dos botones en el lado izquierdo del dashboard.

Por otro lado, las segmentaciones disponibles son:
- Estado de la certificación
- Año en el que se concedió la certificación
- Tipo de registro
- Provincia
- Normativa
- Mes en el que se concedió la certificación




## Informe explicativo del análisis

En los KPIs principales, se observa que el porcentaje de certificaciones A de emisiones es ligeramente superior al de certificaciones A de consumo, dentro de las certificaciones vigentes, que son un 85,8% de las certificaciones totales.

En la gráfica de evolución de certificaciones se aprecia un claro descenso de las certificaciones G y F, a la par que aumentan las D y E, para establecerse en los últimos años. Las certificaciones A y B crecen en los últimos años. Este análisis es similar para consumo y para emisiones.

Por provincias, Alicante es la que más certificados tiene registrados, a continuación Valencia (cifras muy similares) y por último Castellón, con un número visiblemente inferior. Se aprecia también que tanto en consumo como en emisiones, el porcentaje de certificaciones A es superior en Valencia. No se aprecian diferencias significativas entre las certificaciones de consumo y las de emisiones.

En las certificaciones de las viviendas, tanto en consumo como en emisiones, destaca claramente la letra E. Sin embargo, si se filtra por Tipo de registro" = "Nuevo", destacan las letras A y B. Será interesante evaluar la evolución de las viviendas en los próximos años.

Por último, se observan muchas diferencias en el gráfico de certificaciones A y G por tipo de uso del inmueble, en función de los filtros seleccionados. Sé destaca el hecho de que en instalaciones deportivas, hoteles, hospitales, edificios comerciales y centros de enseñanza, el porcentaje de certificaciones A se impone a las G.


Este es un resumen de los datos que se pueden obtener, y unos resultados generales. En función del tipo de estudio que se quiera realizar y el objetivo, se pueden combinar distintos filtros para profundizar en el análisis.




## Próximos pasos

- Mejorar la variable "AÑO_CONS": en lugar de considerarla vacía, crear un programa en Python que busque cada registro en el catastro y rellene el año de construcción correctamente.
- Plantear este análisis a nivel nacional.
- Dado que en el documento Descripción datos de descarga se indica que los datos se actualizan mensualmente y pueden variar entre descargas (expedientes eliminados, actualizados, correcciones fruto de inspecciones,...), una posible mejora sería tratar de crear un programa que detecte los cambios entre descargas, antes de volver a actualizar el dashboard. Por ejemplo, descargando los datos en otro excel y comparando los registros.





## Autora

Iciar González
