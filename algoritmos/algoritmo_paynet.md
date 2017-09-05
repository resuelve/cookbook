## Algoritmo Paynet (1 Dígito Verificador)


### Procedimiento para calcular el Dígito Verificador

	

#### DATOS NECESARIOS PARA EL CALCULO:


#### Ejemplo:

Si la Referencia es igual a: ```00003Y10101396539```


#### Tabla de relación Paynet:
	
|  Valor | Reemplazar por: |
|---|---|
| A | 1 |
| J | 1 | 
| 1 | 1 |
| B | 2 |
| K | 2 |
| S | 2 |
| 2 | 2 |
| C | 3 |
| L | 3 |
| T | 3 |
| 3 | 3 |
| D | 4 |
| M | 4 |
| U | 4 |
| 4 | 4 |
| E | 5 |
| N | 5 |
| V | 5 |
| 5 | 5 |
| F | 6 |
| O | 6 |
| W | 6 |
| 6 | 6 |
| G | 7 |
| P | 7 |
| X | 7 |
| 7 | 7 |
| H | 8 |
| Q | 8 |
| Y | 8 |
| 8 | 8 |
| I | 9 |
| R | 9 |
| Z | 9 |
| 9 | 9 |
| 0 | 0 |	


1. Se hace un mapeo inicial, se reemplaza cada uno de los valores de la referencia por los de la tabla enterior.

	| 0 | 0 | 0 | 0 | 3 | Y | 1 | 0 | 1 | 0 | 1 | 3 | 9 | 6 | 5 | 3 | 9 |
	|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
	| 0 | 0 | 0 | 0 | 3 | 8 | 1 | 0 | 1 | 0 | 1 | 3 | 9 | 6 | 5 | 3 | 9 |



2. De derecha a izquierda se van multiplicando cada uno de los dígitos por los números 2 y 1, siempre iniciando la
secuencia con el número 2 aun cuando el número a multiplicar sea 0 deberá tomarse en cuenta. Si el resultado de la
multiplicación es mayor a 9, se deberán sumar las unidades y las decenas, de tal forma que solo se tenga como resultado un
número menor 0 igual a 9.


	| 0   | 0  | 0  | 0  | 3  | 8  | 1   | 0  | 1  | 0  | 1  | 3  | 9     | 6  | 5      | 3  | 9      |
	|-----|----|----|----|----|----|-----|----|----|----|----|----|-------|----|--------|----|--------|
	| 0*2 | 0*1| 0*2|0*1 |3*2 |8*1 |1*2  |0*1 |1*2 |0*1 |1*2 |3*1 |9*2    |6*1 |5*2     |3*1 |9*2     |
	|  0  |  0 |  0 | 0  | 6  |  8 | 2   | 0  | 2  | 0  | 2  | 3  |**1+8**| 6  | **1+0**| 3  |**1+8** |
	|     |    |    |    |    |    |     |    |    |    |    |    |    9  |    |    1   |    |    9   | 

 
3. Se suman todos los resultados de las multiplicaciones del punto 1.

	***0 + 0 + 0 + 0 + 6 + 8 + 2 + 0 + 2 + 0 + 2 + 3 + 9 + 6 + 1 + 3 + 9  = 51***

	
4. El resultado de la suma indicada en el punto 2, deberá restársele a la decena superior mas próxima. El resultado de esta substracción será el dígito verificador.
	
	***60 - 51 = 9***
	
	Dígito Verificador: ***9***



5. A la referencia se le agregara el dígito verificador y esa será la línea de captura que recibirá el cajero en ventanilla.


	Referencia Completa: 00003Y10101396539***9***


Ejemplo en python:

```python

import math

def check_digit(reference):
    isTwo = True
    sum_elements = 0
    mapPaynet = {
        'A': 1, 
        'J': 1, 
        '1': 1,
        'B': 2, 
        'K' : 2, 
        'S' : 2, 
        '2' : 2,
        'C' : 3, 
        'L' : 3, 
        'T' : 3, 
        '3' : 3,
        'D' : 4, 
        'M' : 4, 
        'U' : 4, 
        '4' : 4,
        'E' : 5,
        'N' : 5, 
        'V' : 5, 
        '5' : 5,
        'F' : 6, 
        'O' : 6, 
        'W' : 6, 
        '6' : 6,
        'G' : 7, 
        'P' : 7, 
        'X' : 7, 
        '7' : 7,
        'H' : 8, 
        'Q' : 8, 
        'Y' : 8, 
        '8' : 8,
        'I' : 9, 
        'R' : 9, 
        'Z' : 9, 
        '9' : 9,
        '0' : 0	
    }

    for n in reversed(reference):
        number =  mapPaynet[n]
        if isTwo:
            isTwo = False
            result = number * 2
        else:
            isTwo = True
            result = number * 1

        if result > 9:
           ntemp = list(str(result))
           result = int(ntemp[0]) + int(ntemp[1])

        sum_elements =  sum_elements + result

    round_up = int(math.ceil(sum_elements / 10.0)) * 10
    digit = round_up - sum_elements

    if digit == 10:
        digit = 0

    return digit


```
