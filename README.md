# agnieszkatruty-github.io
import numpy as np 
import matplotlib.pyplot as plt
import rasterio 

# Obliczanie NDVI
def calculate_ndvi(nir_band, red_band):
    np.seterr(divide='ignore', invalid='ignore')  # Ignorowanie błędów dzielenia przez 0
    ndvi = (nir_band.astype(float) - red_band.astype(float)) / (nir_band + red_band)
    return ndvi

# Ładowanie obrazu
def load_image(file_path):
    with rasterio.open(file_path) as src:
        red_band = src.read(3)  # Pasmo czerwone (RED)
        nir_band = src.read(4)  # Pasmo bliskiej podczerwieni (NIR)
    return red_band, nir_band

# Analiza NDVI i wizualizacja
def ndvi_analysis(before_image, after_image):
    red_before, nir_before = load_image(before_image)
    red_after, nir_after = load_image(after_image)
    
    ndvi_before = calculate_ndvi(nir_before, red_before)
    ndvi_after = calculate_ndvi(nir_after, red_after)
    
    change_in_ndvi = ndvi_after - ndvi_before
    
    fig, axes = plt.subplots(1, 3, figsize=(18, 6))
    axes[0].imshow(ndvi_before, cmap='YlGn')
    axes[0].set_title('NDVI before')
    
    axes[1].imshow(ndvi_after, cmap='YlGn')
    axes[1].set_title('NDVI after')
    
    im = axes[2].imshow(change_in_ndvi, cmap='coolwarm', vmin=-1, vmax=1)
    axes[2].set_title('Change NDVI')
    
    fig.colorbar(im, ax=axes[2], orientation='horizontal', fraction=0.046, pad=0.04)
    plt.show()

# Ścieżki do obrazów
file_path_1 = r"C:\Users\truty\Desktop\PROGRAMMING\x1.TIF"
file_path_2 = r"C:\Users\truty\Desktop\PROGRAMMING\x2.TIF"

# Wywołanie analizy NDVI
ndvi_analysis(file_path_1, file_path_2)
