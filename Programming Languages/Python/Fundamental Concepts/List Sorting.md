El **algoritmo de ordenamiento por burbuja** itera repetidamente sobre la lista comparando pares adyacentes y permutándolos si están en el orden incorrecto, repitiendo pasadas hasta que no se realizan intercambios (lista ordenada). En Python se implementa con dos bucles anidados: **el externo controla el número de pasadas** $(n‑1)$ y **el interno recorre índices** 0..n-i-2 comparando y haciendo swaps con una asignación múltiple $(a[j], a[j+1] = a[j+1], a[j])$; añadiendo una variable/flag booleana que detecta ausencia de intercambios se puede terminar antes. 

~~~Python
def main(): 
	my_list = []
	swapped = True
	num = int(input("How many elements do you want to order?: "))
    
	for i in range(num): 
		val = float(input("Enter the element of the list: "))
		my_list.append(val)
	
	while swapped: 
		swapped = False
		for i in range(len(my_list) - 1): 
			if my_list[i] > my_list[i + 1]: 
				my_list[i], my_list[i + 1] = my_list[i + 1], my_list[i] 
				swapped = True
	
	return my_list	
		
if __name__ == "__main__": 
	sorted_list = main()
	print("\List: ")
	print(sorted_list)
~~~

Python permite ordenar la lista con el método `.sort()`

~~~Python
my_list[3,2,5,8,4,7,1]
my_list.sort()
print(my_list)

# [1, 2, 3, 4, 5, 7, 8]
~~~

También hay un método de lista llamado `reverse()`, que se puede usar para invertir la lista

~~~Python
lst = [5, 3, 1, 2, 4]
print(lst)

lst.reverse()
print(lst)  

# [4, 2, 1, 3, 5]
~~~