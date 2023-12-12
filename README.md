# Trabajo-Ordinario

Maestro: Sebastián Gonzales Zepeda


Autores: 
Hernán Córdova Araiza

Grado y Grupo: 3ºB

Resumen

En este proyecto analizaremos los ciclones y sus categorías, principalmente la velocidad del viento durante los últimos tres años. Para ello, obtenemos información de sitios web fiables que proporcionan las fechas en las que estos ciclones tocaron tierra en un lugar concreto. Disponemos de tablas de datos en Excel sobre la historia de estos ciclones, principalmente su velocidad. Este código nos da la información que necesitamos porque con 100 datos tomaría mucho tiempo calcular cada categoría una por una.


Introducción

Las velocidades de los ciclones en México son un fenómeno meteorológico muy significativo debido a la ubicación geográfica del país, que suele verse afectado por huracanes y tormentas tropicales. Comprender la velocidad de estos ciclones es esencial para evaluar su impacto potencial en el viento, las precipitaciones y las marejadas ciclónicas y para implementar estrategias de preparación y gestión de riesgos.


Desarrollo

Primero, recuperamos los datos de velocidad (km/h) de Internet y los agrupamos en una tabla de datos de Excel ordenada para escribir el código para determinar la categoría de cada dato en la biblioteca de pandas y proporcionar una nueva columna llamada "Categoría" que muestra la categoría de cada ciclón, porque nuestro rango es "235", indica que hay 235 km/h entre la velocidad mínima y la velocidad máxima, lo que indica que tenemos estos 100 datos, es muy pequeño y la velocidad es muy alto.




Manejo de datos
El manejo de datos que tenemos lo tenemos organizado en Excel, donde se llevara a cabo este proyecto, por eso haremos un código que tenga acceso a esta tabla de datos y nos pueda dar la información que deseamos.


Código

import pandas as pd
import matplotlib.pyplot as plt
import sys

def cargar_datos_desde_excel(nombre_archivo):
    try:
        return pd.read_excel(nombre_archivo)
    except FileNotFoundError:
        print(f"Error: No se encontró el archivo '{nombre_archivo}'")
        return None

def categorizar_ciclon(velocidad_viento):
    if velocidad_viento < 74:
        return "Categoría 1: Tormenta tropical"
    elif velocidad_viento < 96:
        return "Categoría 2: Huracán"
    elif velocidad_viento < 111:
        return "Categoría 3: Huracán mayor"
    elif velocidad_viento < 130:
        return "Categoría 4: Huracán muy fuerte"
    else:
        return "Categoría 5: Huracán extremadamente fuerte"

def visualizar_datos(datos, columna_velocidad, columna_muertes):
    # Seleccionar las velocidades más fuertes
    velocidades_fuertes = datos.nlargest(10, columna_velocidad)

    # Seleccionar los ciclones más mortales
    ciclones_mortales = datos.nlargest(5, columna_muertes)

    # Graficar las velocidades más fuertes y los ciclones más mortales
    plt.figure(figsize=(12, 8))
    plt.bar(velocidades_fuertes['NombreCiclon'], velocidades_fuertes[columna_velocidad], color='skyblue', label='Velocidades Fuertes', edgecolor='black')
    plt.bar(ciclones_mortales['NombreCiclon'], ciclones_mortales[columna_velocidad], color='salmon', label='Ciclones Mortales', edgecolor='black')

    # Añadir leyenda y etiquetas
    plt.xlabel('Nombre del Ciclón')
    plt.ylabel('Velocidad del Viento (km/h)')
    plt.title('Top 10 Velocidades de Viento y Ciclones Mortales')
    plt.xticks(rotation=45, ha='right')
    plt.legend()

    # Mostrar la gráfica
    plt.tight_layout()
    plt.show()

def main(argv):
    if len(argv) != 2:
        print("Uso: python script.py <nombre_archivo.xlsx>")
        sys.exit(1)

    nombre_archivo = argv[1]
    datos_ciclones = cargar_datos_desde_excel(nombre_archivo)

    if datos_ciclones is not None:
        # Categorizar los ciclones
        datos_ciclones['Categoria'] = datos_ciclones['VelocidadViento'].apply(categorizar_ciclon)

        # Visualizar datos
        visualizar_datos(datos_ciclones, 'VelocidadViento', 'NumeroMuertes')

# Llamada al programa principal
if __name__ == "__main__":
    main(sys.argv)


Este código que elaboramos para el proyecto lee los datos proporcionados en el archivo Excel sobre las velocidades de viento que generan los ciclones, calcula la categoría para cada velocidad de cada ciclón y agrega una nueva columna llamada “categoría” al archivo Excel, actualizando la información de ese archivo y guardando la información en uno nuevo.

