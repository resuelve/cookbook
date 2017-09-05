## Algoritmo Bancomer 00 (1 Dígito Verificador)


###Procedimiento para calcular el Dígito Verificador

#### DATOS NECESARIOS PARA EL CALCULO:

Referencia de 1 a 19 Dígitos

Ejemplo:

Si la Referencia es igual a: ```18000359700002387 ```

1. De derecha a izquierda se van multiplicando cada uno de los dígitos por los números 2 y 1, siempre iniciando la
secuencia con el número 2 aun cuando el número a multiplicar sea 0 deberá tomarse en cuenta. Si el resultado de la
multiplicación es mayor a 9, se deberán sumar las unidades y las decenas, de tal forma que solo se tenga como resultado un
número menor 0 igual a 9.


	|1    |8   |0   |0   |0   |3   |5   |9   |7   |0   |0   |0   |0   |2   |3   |8   |7   |
|-----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| 1*2 | 8*1| 0*2|0*1 |0*2 |3*1 |5*2 |9*1 |7*2 |0*1 |0*2 |0*1 |0* 2|2*1 |3*2 |8*1 |7*2 |
|  2  |  8 |  0 | 0  | 0  |  3 |**1+0**|  9 |**1+4**|0   | 0  | 0  | 0  | 2  | 6  |8   |**1+4** |
|     |    |    |    |    |    |    1  |    |   5   |    |    |    |    |    |    |    |    5   | 

 
2. Se suman todos los resultados de las multiplicaciones del punto 1.

	***2 + 8 + 0 + 0 + 0 + 3 + 1 + 9 + 5 + 0 + 0 + 0 + 0 + 2 + 6+ 8 + 5 = 49***

	
3. El resultado de la suma indicada en el punto 2, deberá restársele a la decena superior mas próxima. El resultado de esta substracción será el dígito verificador.
	
	***50 - 49 = 1***
	
	Dígito Verificador: ***1***



4. A la referencia se le agregara el dígito verificador y esa será la línea de captura que recibirá el cajero en ventanilla.


	Referencia Completa: 18000359700002387***1***


Ejemplo en python:

```python
	
import math

"""
    Algoritmo Bancomer 00 (1 Digito Verificador)
    Procedimiento para calcular el Digito Verificador
    DATOS NECESARIOS PARA EL CALCULO:
    Referencia de 1 a 19 Digitos
"""

def algoritmo00(reference):
    isTwo = True
    sum_elements = 0
    for n in reversed(reference):
        if isTwo:
            isTwo = False
            result = int(n) * 2
        else:
            isTwo = True
            result = int(n) * 1

        if result > 9:
           ntemp = list(str(result))
           result = int(ntemp[0]) + int(ntemp[1])

        sum_elements =  sum_elements + result

    round_up = int(math.ceil(sum_elements / 10.0)) * 10
    digit = round_up - sum_elements

    return digit

```