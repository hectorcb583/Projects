# Projects
Scripts for solving everyday problems.

import os
import zipfile
import pypdf
from PIL import Image

comics = [""]
carpeta_origen = ""

for archivo_zip in comics:
    #os.chdir(carpeta_origen) # Cambiamos a la carpeta de descargas para facilitar los nombres
    nombre_comic = os.path.splitext(os.path.basename(archivo_zip))[0] # Obtenemos el nombre de los comics
    
    carpeta_destino = f"/Users/{nombre_comic}"
    
    carpeta_pdfs = f"/Users/{nombre_comic}_pdfs"

    carpeta_final = f"{carpeta_pdfs}/"+"{}.pdf"
    
    os.makedirs(carpeta_destino, exist_ok=True)
    os.makedirs(carpeta_pdfs, exist_ok=True)

    with zipfile.ZipFile(f"{archivo_zip}" , "r" ) as my_zip:
       #print(my_zip.namelist())  ver el formato de las imagenes
       my_zip.extractall(f"{carpeta_destino}")

    os.chdir(carpeta_destino)

    for f in os.listdir("."):
        if f.lower().endswith((".jpg", ".jpeg", ".png", ".webp")):
            i = Image.open(f)
            fn, fext = os.path.splitext(f)
            i.save(carpeta_final.format(fn))
    
    
    
    archivos = sorted(
        [f for f in os.listdir(carpeta_pdfs) if f.endswith(".pdf")],
        key=lambda x: int(os.path.splitext(x)[0])
    )
    
    print(archivos)  # para comprobar el orden
    
    merger = pypdf.PdfWriter()
    
    for archivo in archivos:
        ruta_completa = os.path.join(carpeta_pdfs, archivo)
        merger.append(ruta_completa)
    
    merger.write(f"/Users/{nombre_comic}.pdf")
    merger.close()
