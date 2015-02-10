# Chemical Warehouse

## Notas o ideas para desarrollar el sistema de inventarios del almacén de sustancias químicas de la UPEL.

Preguntas a responder:
1) ¿Cuántos envases hay?
2) ¿Cuanto hay en cada envase?
3) ¿Donde están?


## Registrar Nueva Familia de químicos

Url: new-family :: new

Acción:
<registrar nueva familia>

Dependencias:
  - NULL

Formulario
  - Nombre

Base De Datos
  - id
  - name
  - created
  - modified

Vista
  - text input -> nombre

## Registrar Nuevo Fabricante 

Url: new-manufacturer

Acción:
<registrar nuevo fabricante>

Dependencias:
  - NULL

Formulario
  - Nombre

Base De Datos
  - id
  - name
  - created
  - modified

Vista
  - text input -> nombre

## Registrar Nueva Sustancia

Acción:
<registrar nueva sustancia>

Dependencias:
  - Familias

Formulario:
  - Familia a la que pertenece la sustancia química.
  - Nombre de la sustancia.
  - NR numero de reactivo o numero de control , permite determinar una sustancia por un numero corto fácil de recordar.

Base de datos:
  - id
  - family-id
  - name
  - reactive-number
  - created
  - modified

Vista:

  - Selector -> Familia.
  - text input -> nombre
  - text input -> numero del reactivo

## Registrar Nueva presentación

Acción:
<registrar nueva presentación>

Dependencia:
  - Sustancia
  - Fabricante

Formulario:
  - En su presentación de [   ]
  - Unidad métrica:  @-> gramos, O-> mililitros
  - Laboratorio Fabricante de la sustancia química

Base de datos:
  - id
  - substance-id
  - manufacturer-id
  - presentation
  - metric-unit

Vista:

Menu >> Familia >> Sustancia = sustancia_id

  - Selector -> Fabricante.
  - text input -> presentación
  - radio input -> unidad métrica

## Registrar Un Nuevo Envase

Acción:
<registrar un lote nuevo o envase>

Menu Acordeón

```
Menu >>
    Familias >>
        sustancias >> onClick{

            (VISTA ADMINISTRATIVA){

                (si hay envases se muestra){

                    Los datos precargados >>

                        sistema-> nombre de la familia: Acetato
                        sistema-> nombre de la sustancia: sodio
                        sistema-> nr: 427
                        sistema-> presentación: 200 g
                        sistema-> unidad_métrica: g
                        sistema-> fabricante: Merck
                        sistema-> Total de envases: 3
                        sistema-> Total de gramos: 490g

                    El lote de sustancias >>

                             C EN 	    Estante	    Tramo
                        1->  190 g      1		    1         <Seleccionar>               // Envase->id
                        2->  200 g      1		    1         <Seleccionar>               // Envase->id
                        3->  100 g      2		    1         <Seleccionar>               // Envase->id


                            ACCIÓN: <Seleccionar> Modal con los siguientes campos:

                            [C EN]
                            [Estante]
                            [Tramo]

                            <Actualizar> <Borrar>




                    La opción de registrar nuevo lote >>

                        <registrar un lote nuevo>
                            [Cantidad]
                            [Estante]
                            [Tramo]

                }

                (si no hay envases se muestra){

                    <<se informa que no hay envases para esta presentación>>

                    Los datos precargados >>
                        sistema-> nombre de la familia: Acetato
                        sistema-> nombre de la sustancia: sodio
                        sistema-> nr: 427
                        sistema-> presentación: 200 g
                        sistema-> unidad_metrica: g
                        sistema-> fabricante: Merck
                        sistema-> Total de envases: 0
                        sistema-> Total de gramos: 0 g

                    La opción de registrar nuevo lote >>

                        <registrar un lote nuevo>
                            [Cantidad]
                            [Estante]
                            [Tramo]

                }
            }

            (VISTA USUARIO){

                (si hay envases se muestra){

                    Los datos precargados >>

                        sistema-> nombre de la familia: Acetato
                        sistema-> nombre de la sustancia: sodio
                        sistema-> nr: 427
                        sistema-> presentación: 200 g
                        sistema-> unidad_metrica: g
                        sistema-> fabricante: Merck
                        sistema-> Total de envases: 3
                        sistema-> Total de gramos: 490g

                    El lote de sustancias >>

                             C EN 	    Estante	    Tramo
                        1->  190 g      1		    1
                        2->  200 g      1		    1
                        3->  100 g      2		    1


                }

                (si no hay envases se muestra){

                    <<<se informa que no hay envases para esta presentación>>>

                    Los datos precargados >>
                        sistema-> nombre de la familia: Acetato
                        sistema-> nombre de la sustancia: sodio
                        sistema-> nr: 427
                        sistema-> presentación: 200 g
                        sistema-> unidad_metríca: g
                        sistema-> fabricante: Merck
                        sistema-> Total de envases: 0
                        sistema-> Total de gramos: 0 g


                }
            }

}
```


## Selección de categoría (Familia->Sustancia). 

```
containers
  -id
  -category_id
  -laboratoy_id
  -stand
  -section
  --deleted
  --created
  --modified

control_numbers
  -id
  -category_id
  -control_number
  --deleted
  --created
  --modified

laboratories
  -id
  -name
  --deleted
  --created
  --modified
```


# 2 nota 

```
Registrar nueva Sustancia: registro generico de la sustancia química. 
  -> Familia a la que pertenece la sustancia química.
  -> Nombre de la sustancia
  -> NR numero de reactivo, numero de control.
  -> Código de barra:  

Registrar nueva presentación: 
  -> En su presentación de [   ]    
  -> Unidad métrica:  @-> gramos, O-> mililitros
  -> Laboratorio Fabricante de la sustancia química 
  -> Código de barra:  
```



```
Administrativo.

Se lee el código de barras.

	si existe el código de barras.
		El sistema busca sustancias en stock
			si hay envases se muestra 
			
				Los datos precargados
					sistema-> nombre de la familia: Acetato
					sistema-> nombre de la sustancia: sodio
					sistema-> nr: 427
					sistema-> presentación: 200g
					sistema-> fabricante: Merck
					sistema-> Código de Barras de la presentación: {232003222132} 
				
				El lote de sustancias 
					 	 	
					 	 C EN 	  Estante	Tramo		Código de Barras del envase  	
					1->  [190] g  [1]		[1]			{32165813215132}
					2->  [200] g  [1]		[1]			{25475562131365}
					3->  [100] g  [2]		[1]			{32165813352135}
					
					Información: 
						
						Totales
						envases: 3  
						gramos:  490 g
					
				La opción de registrar nuevo lote 															
					<registrar nuevo lote>
						se indica la cantidad 
						se indica el lugar donde va estar, numero del estante y el tramo que ocupa.	
						fin

			si no hay envases se muestra 
				
				<<se informa que no hay envases>> 				
				
				Los datos precargados
					sistema-> nombre de la familia: Acetato
					sistema-> nombre de la sustancia: sodio
					sistema-> nr: 427
					sistema-> presentación: 200g
					sistema-> fabricante: Merck
					sistema-> Código de Barras de la sustancia: {232003222132} 
						
				La opción de registrar nuevo lote 															
					<registrar nuevo lote>

						usuario-> cantidad: 10	-> int
						usuario-> estante: 1	-> varchart
						usuario-> tramo: 1 	-> varchart

						se indica la cantidad 
						se indica el lugar donde va estar, numero del estante y el tramo que ocupa.	
						fin		
								
				
	no existe el código de barras.
		Se informa que se debe pre-cargar los datos genericos de la nueva presentación de la sustancia química. 
		fin
```


Generar un Solicitud. 

```
El solicitante busca es el código de barra del envase y especifica la cantidad que necesita.

Menu->Familia->Sustancia 

sistema-> nombre de la familia: Acetato
sistema-> nombre de la sustancia: sodio
sistema-> nr: 427
```

```
       C EN 	  Estante	Tramo  fabricante	Código de Barras del envase  	
	1->  [190] g  [1]		  [1]	 				{32165813215132}
	2->  [200] g  [1]		  [1]					{25475562131365}
	3->  [100] g  [2]		  [1]					{32165813352135}
```


permite manejar múltiples  formatos de medida como unidad, metro, kilo, litro, gramo, etc.