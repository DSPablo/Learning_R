# Learning_R
	# Algunas definiiciones...
	# Un paquete es un conjunto de funciones y datos (y algunas cosas más).
	# Una función es algo así como una receta: Le doy los ingredientes, hace "algo", y me devuelve el resultado.
	# Un data frame es una colección rectangular de variables (columnas) y observaciones (filas).
	# pintamos las líneas 10 a la 16 y ponemos "Run"
	install.packages("tidyverse")
	library(tidyverse)
	install.packages("devtools")
	devtools::install_github("cienciadedatos/datos") # si tienen que elegir una opcion, la 1
	library(datos)
	# Para que este texto no quede fuera de la pantalla, podemos ir al Menú "Tools" -> "Global Options..." -> en "Code" marcar la opción "Soft-wrap R source files".
	# Vamos a usar el dataframe "millas", que tiene datos de autos. Vamos a confiar en que están "bien", normalmente gran parte del trabajo del Data Scientist es asegurarse de que los datos no tienen errores y tienen el formato que necesitamos. Cómo hacerlo se ve más adelante en el libro!
	millas
	# algunas cosas útiles
	?millas # ayuda sobre el dataframe millas
	dim(millas) # cantidad de filas y columnas del dataframe millas
	nrow(millas) # cantidad de filas del dataframe millas
	ncol(millas) # cantidad de columnas del dataframe millas
	names(millas) # nombres de las variables
	head(millas) # muestra las primeras observaciones
	summary(millas) # muestra un resumen de las variables, si son numeros o texto, etc.
	View(millas) # muestra los datos como una planilla de cálculo.
	#~~~~~~~~ ESTÉTICAS (PROPIEDADES VISUALES) ~~~~~~~~#
	# Intentemos contestar la pregunta
	# ¿Los automóviles con motores grandes consumen más combustible que los automóviles con motores pequeños? ¿Cómo es la relación entre el tamaño del motor y la eficiencia del combustible? ¿Es positiva? ¿Es negativa? ¿Es lineal o no lineal?
	# Entre las variables de millas se encuentran:
	?millas
	# cilindrada: tamaño del motor del automóvil, en litros.
	# autopista: eficiencia del uso de combustible de un automóvil en carretera, en millas por galón. Al recorrer la misma distancia, un automóvil de baja eficiencia consume más combustible que un automóvil de alta eficiencia.
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista))
	# El gráfico muestra una relación negativa entre el tamaño del motor (cilindrada) y la eficiencia del combustible (autopista). En otras palabras, los vehículos con motores grandes usan más combustible.
	# Qué pasa con los puntos que se escapan a la tendencia?
	# Puedo incorporar un parámetro estético (propiedad visual) adicional al gráfico para intentar responder esta pregunta!
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, color = clase))
	# Y si le asignamos a otra estética, como el tamaño (size)?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, size = clase))
	# Es una buena idea?
	# Y otras como la transparencia (alpha)? O la forma (shape)?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, alpha = clase))
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, shape = clase))
	# Acá qué pasa con las "suv"?
	# Una vez que mapeamos variables con propiedades estéticas, el paquete ggplot2 selecciona una escala razonable para usar con la estética elegida y construye una leyenda que explica la relación entre niveles y valores.
	############################ EJERCICIO ############################
	# Asigna una variable continua a color, size, y shape. ¿Cómo se comportan estas estéticas de manera diferente para variables que son texto y las que son números?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, color = ciudad))
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, size = ciudad))
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, shape = ciudad))
	# Notas: Las variables continuas son las que aparecen como <int> o <dbl> cuando ejecutamos millas.
	# También puedo usar names(millas), summary(millas) para tener más información a la hora de elegir las variables.
	######################################################################
	# También puedes fijar las propiedades estéticas de tu geom manualmente. Por ejemplo, podemos hacer que todos los puntos del gráfico sean azules (fuera de aes()):
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista), color = "blue")
	############################ EJERCICIO ############################
	# ¿Se puede mapear la misma variable a múltiples estéticas? Contesta con un ejemplo.
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, color = autopista, size = autopista))
	#####################################################################
	# Errores comunes:
	#
	# - Comprueba la parte izquierda de tu consola: si es un +, significa que R no cree que hayas escrito una expresión completa y está esperando que la termines. En este caso, normalmente es más fácil comenzar de nuevo desde cero presionando ESCAPE para cancelar el procesamiento del comando actual.
	#
	# - colocar el + en el lugar equivocado: debe ubicarse al final de la línea, no al inicio
	#
	# - ?nombre_de_la_funcion
	#
	# - Cómo buscar ayuda? En inglés es más fácil... intenta buscar el mensaje de error, ya que es probable que otra persona haya tenido el mismo problema y haya obtenido ayuda en línea.
	# - Está StackOverflow en español donde se pueden encontrar respuestas en español!
	# - Usando Google Chrome, se puede traducir toda la página. Entonces puedo buscar el mensaje de error que es en inglés y después traducir toda la página. No es la mejor traducción, pero nos puede dar una idea.
	# - Telegram del Grupo de Usuarios de R en Uruguay!
	# - En Twitter usando #rstatsES
	############################ EJERCICIO ############################
	# ¿Qué ocurre si se asigna o mapea una estética a algo diferente del nombre de una variable, como aes(color = cilindrada < 5)?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, color = cilindrada < 5))
	#####################################################################
	#~~~~~~~~ SEPARAR EN FACETAS ~~~~~~~~#
	# Una forma de agregar variables adicionales es con las estéticas. Otra forma particularmente útil para las variables categóricas consiste en dividir el gráfico en facetas, es decir, sub-gráficos que muestran cada uno un subconjunto de los datos.
	# separando por una variable
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista)) +
	facet_wrap(~ clase)
	# separando por dos variables
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista)) +
	facet_grid(traccion ~ cilindros)
	############################ EJERCICIOS ############################
	# ¿Qué significan las celdas vacías que aparecen en el gráfico generado usando facet_grid(traccion ~ cilindros)? ¿Cómo se relacionan con este gráfico?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = traccion, y = cilindros))
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista)) +
	facet_grid(traccion ~ cilindros)
	Es que no hay datos que cumplan esas condiciones. Podemos verlo usando View(millas) y filtrando los datos.
	# ¿Qué ocurre si intentas separar en facetas una variable continua?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista)) +
	facet_wrap(~ cilindrada)
	####################################################################
	# Comparemos estos dos gráficos. Cuál es mejor para contestar la pregunta que teníamos?
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista, color = clase))
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista)) +
	facet_wrap(~ clase, nrow = 2)
	#~~~~~~~~ OBJETOS GEOMÉTRICOS ~~~~~~~~#
	# Un geom es el objeto geométrico usado para representar datos de forma gráfica.
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista))
	ggplot(data = millas) +
	geom_smooth(mapping = aes(x = cilindrada, y = autopista))
	ggplot(data = millas) +
	geom_bar(mapping = aes(x = cilindrada))
	# no todas las estéticas funcionan con todos los geom.
	ggplot(data = millas) +
	geom_smooth(mapping = aes(x = cilindrada, y = autopista, linetype = traccion))
	ggplot(data = millas) +
	geom_smooth(mapping = aes(x = cilindrada, y = autopista, color = traccion))
	#~~~~~~~~ MÚLTIPLES GEOMS ~~~~~~~~#
	ggplot(data = millas) +
	geom_point(mapping = aes(x = cilindrada, y = autopista)) +
	geom_smooth(mapping = aes(x = cilindrada, y = autopista))
	# o... para no repetir código...
	ggplot(data = millas, mapping = aes(x = cilindrada, y = autopista)) +
	geom_point() +
	geom_smooth()
	ggplot(data = millas, mapping = aes(x = cilindrada, y = autopista)) +
	geom_point(mapping = aes(color = clase)) +
	geom_smooth()
	# Podemos cambiar el sistema de coordenadas
	bar <- ggplot(data = millas) +
	geom_bar(
	mapping = aes(x = clase),
	show.legend = FALSE,
	width = 1
	) +
	theme(aspect.ratio = 1) +
	labs(x = NULL, y = NULL)
	bar + coord_polar()
	# se pueden hacer mapas como este también! https://d4tagirl.com/2017/05/visualizing-r-ladies-growth
	# ggplot2 proporciona más de 40 geoms y los paquetes de extensión proporcionan aún más (consulta https://exts.ggplot2.tidyverse.org/ para obtener una muestra). La mejor forma de obtener un panorama completo sobre las posibilidades que brinda ggplot2 es consultando la hoja de referencia (o cheatsheet), que puedes encontrar en https://github.com/rstudio/cheatsheets/raw/master/translations/spanish/ggplot2.pdf. Para obtener más información sobre un tipo dado de geoms, usa la ayuda: ?geom_smooth.
	############################ EJERCICIO ############################
	# Nota: Para hacer estos ejercicios me va a servir mirar la guía rápida de "Visualización de Datos usando ggplot2" https://github.com/rstudio/cheatsheets/raw/master/translations/spanish/ggplot2.pdf
	# Cómo puedo girar (o voltear) las coordenadas en el gráfico bar? 
	bar + coord_flip()
	# Y si quiero cambiar el color de las barras para que sea diferente según la clase? (Quizás sea necesario buscar ayuda para responder esto).
	ggplot(data = millas) +
	geom_bar(
	mapping = aes(x = clase, fill = clase),
	show.legend = FALSE,
	width = 1
	) +
	theme(aspect.ratio = 1) +
	labs(x = NULL, y = NULL)
	# Y si quiero cambiar el "tema"?
	bar + theme_minimal()
	###################################################################### 
	# En el libro después se trata cómo limpiar y ordenar datos y muchas cosas más, y lo bueno es que ya tienen el primer capítulo terminado!
	# R EN URUGUAY
	# R-Ladies Montevideo - https://www.meetup.com/rladies-montevideo/
	# Grupo de Usuaries de R en Uruguay - GURU - https://www.meetup.com/GURU-mvd/
> 
![image](https://user-images.githubusercontent.com/79336445/111338729-c834df00-863c-11eb-9600-243d9fee76ad.png)
