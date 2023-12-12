# Trabajo-Ordinario

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
