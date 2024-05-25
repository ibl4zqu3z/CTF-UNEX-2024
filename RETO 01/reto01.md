# RETO 01 CTF UNEX 2024

![alt text](images/image-11.png)

Descargo los ficheros que nos dan: Reto01.zip y los descomprimo en mi carpeta de trabajo para este reto.

![alt text](images/image-7.png)

![alt text](images/image-8.png)

Veo que aparece uno que lo reconce como imagen y otro como zip, pero estan incompletos, parece como si hubieran sido "cortados" en tamaños iguales.

## Archivo X

Compruebo el primero viendo que aparece un trozo de una imagen.


![alt text](images/image-6.png)


Uno los ficheros que empiezan por la misma letra con el comando:

```bash
cat x* > x.jpg
```

![alt text](images/image-9.png)

Compruebo el fichero obtenido y veo que es una imagen

![alt text](images/image-12.png)

Es la plaza de españa de sevilla nevada. 

Compruebo Strings

![alt text](images/image-13.png)

Compruebo con binwalk por si hubiera algo escondido.

![alt text](images/image-14.png)

Compruebo con exiftool por si hubiera algo importante en los metadatos.

![alt text](images/image-15.png)

En esta imagen parece que no hay nada reseñable, la dejo a un lado de momento por si fuera necesario mas adelante.

## Archivo Z


Uno los ficheros que empiezan por la misma letra con el comando:

```bash
cat z* > x.jpg
```

![alt text](images/image-10.png)

Descomprimo el zip y tengo una imagen llamada snow.jpg

![alt text](images/image-16.png)

Comprobamos la imagen por si fuera la flag.

![alt text](images/image-17.png)

Tiro un strings -n 10 snow.jpg y aparece algo interesante

![alt text](images/image-5.png)

Nos da una "clave" : `_ApparentlyThisIsAPassw0rd`

Ynos dice que la clave final no esta visible, es decir que la flag esta oculta.

Voy a usar la herramienta "stegsolve" que sirve para ver capas de la imagen y asi poder encontrar algun mensaje oculto en capas.

Esta herramienta la podemos encontrar en: 
 [https://github.com/zardus/ctf-tools/blob/master/stegsolve/install]

Nos indica para instalarla hacer lo siguiente:

```bash
wget http://www.caesum.com/handbook/Stegsolve.jar -O stegsolve.jar
chmod +x stegsolve.jar
mkdir bin
mv stegsolve.jar bin/
```

Una vez realizado solo tengo que ir a la carpeta bin y ejecutar la aplicacion con:

```bash
java -jar stegsolve.jar
```

![alt text](images/image-18.png)

Esto abre una aplicacion en la que tenemos elegir la imagen e ir pasando capa a capa

![alt text](images/image-19.png)

Despues de revisar esta imagen y la anterior no hay nada en las capas que sea la flag.

Como hay una clave busque en google por herramientas que oculta archivos en otros protegiendolos con clave.

La herramienta se llama “Steghide” (https://pkg.kali.org/pkg/steghide)

La instalo en kali:

![alt text](images/image-20.png)

y miro la documentacion de como se usa:

![alt text](images/image-21.png)

Tiro el comando:

steghide extract -sf x.jpg 

Me pide una clave y escribo la clave que encontramos en snow.jpg:  `_ApparentlyThisIsAPassw0rd`

Y me dice que ha escrito los datos en flag.txt,

![alt text](images/image.png)

Miro el fichero con un `cat`

![alt text](images/image-1.png)

Compruebo con `nano` y aparecen unas marcas en rojo como si se hubieran añadido espacios.

![alt text](images/image-22.png)

Abro el fichero con un editor de hexadecimal por si hubiera contenido escondido en valores hexadecimales.

![alt text](images/image-23.png)

Existe un metodo de ocultación de mensajes mediante el método “whitespace”, que no es visible a la vista, pero con cualquier editor de texto en terminal es posible observar en color rojo aquellos lugares en blanco. 

Por lo que estaba ante una ocultacion en espacios en blanco.

Mediante la herramienta stegsnow (casualmente snow como el fichero original desde donde partimos) realizo una extraccion 

![alt text](images/image-3.png)

Y hemos obtenido la flag: **flag{1s_v3ry_H00t}**

![alt text](images/image-24.png)

![alt text](images/image-25.png)

**RETO COMPLETADO!**