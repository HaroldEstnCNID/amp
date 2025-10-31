# AMP
## Requisitos
- Template: "db_template.yaml"

## Aprovisionar Base de Datos
### Creacion de la base de datos
1. Ingresamos a la consola de aws
2. En el buscador buscamos CloudFormation y damos Click sobre el nombre CloudFormation
3. Le damos click en Crear stack, se nos despliega una lista con 2 opciones.
4. Seleccionamos la primera opcion "With new resources (standard)"
5. En Specify template selecionamos "Uplad a template file"
6. Damos click en "Choose file" y desde el gestor de archivos seleccionamos "db_template.yaml"
7. Damos click en Siguiente
8. Definimos un nombre para el stack ejemplo "Stack-AMP-DB"
9. llenamos cada uno de los parametros segun corresponda.
9.1. BastionInstanceType: Especifica el tipo de instancia para el bastion host seleccionamos una de la lista. El bastion nos permitira conectarnos a la base de datos.
9.2. DBInstanceClass: Especifica el tipo de instancia para la base de datos.
9.3. DBName: Especifica el nombre para la base de datos.
9.4. DBPassword: Asignamos una contraseña a la base de datos.
9.5. DBUsername: Definimos un usuario para la base de datos.
9.6. LatestAmiId: Definimos un id de ami o dejamos que por defecto utilize la ultima.
9.7. ProjectTag: Seleccionamos las subnets en la que va a quedar la base de datos(Recomendable subntes privadas sin acceso a intenet)
9.8. ProjectTag: Definimos un tag para los recursos creados
9.9. PublicSubnetIds: Seleccionamos 2 subnets publicas que seran utilizadas por el bastion
9.10. VpcId: Seleccionamos la VPC en la que se desplegaran los recursos.


10. Damos click en "Next"
11. Dejamos todo por defecto y nos desplazamos al final.
12. En Capabilities seleccionamos el checkbox "I acknowledge that AWS CloudFormation might create IAM resources."
13. Damos click en siguiente.
14. Dejamos todo por defecto y nos desplazamos al final.
15. Damos click en "Submit"
16. Sobre el nombre del stack y en Events podemos visualizar el estado de creacion de los recursos.
17. Esperamos a que se cree la base de datos la creacion tarda aproximadamente 5 minutos.
18. En el stak nos dirigimos a outputs y copiamos el comando que se encuentra en HowToConnect que empieza en mysql -h... lo guardamos que lo necesitaremos mas adelante
18. En el buscador de AWS buscamos "RDS" y damos click en "Aurora and RDS"
19. Seccionamos de la lista a la izquierda "Databases"
20. Validamos que nuestra base de datos fue creada correctamente.
### Conexion y transacciones a la base de datos
1. En el buscador de aws buscamos "EC2" y acemos click encima
2. En el apartado derecho seleccionamos en Instances "Instances"
3. En el buscador de instancias buscamos bastion
4. Seleccionamos la instancia.
5. Damos click en "Connect" en la parte superior.
7. Seleccionamos "Session Manager" y damos click en "Connect"
8. Se nos abre una nueva pestaña en la cual podremos conectarnos a la base de datos.
9. pegamos el comando que copiamos en el output de la creacion de la base de datos el comando es similar a "mysql -h [host] -u [user] -p [database]".
10. Damos Enter y nos solicitara la clave que pusimos al crear la base de datos.
11. La escribimos o pegamos y damos enter.
12. Con esto hemos ingresado a la base de datos.
13. Podemos ejecutar lo siguientes comandos.
Revisar tablas creadas.
```
SHOW TABLES;
```

Crear una tabla nueva.
```
CREATE TABLE Persons (
    PersonID INT AUTO_INCREMENT PRIMARY KEY,
    LastName VARCHAR(255),
    FirstName VARCHAR(255),
    Address VARCHAR(255),
    City VARCHAR(255)
);
```

Insertar elementos dentro de la tabla.
```
INSERT INTO Persons (LastName, FirstName, Address, City)
VALUES
    ('Gómez', 'Carlos', 'Calle 10 #45-23', 'Bogotá'),
    ('Martínez', 'Laura', 'Cra 7 #22-15', 'Medellín'),
    ('Rodríguez', 'Andrés', 'Av. Siempre Viva 742', 'Cali');
```

Listar todas las personas de la tabla.
```
Select * from Persons;
```

Eliminar una persona
```
DELETE FROM Persons
WHERE FirstName = 'Laura';
```

Actualizar una persona
```
UPDATE Persons
SET Address = 'Calle 12 #30-10', City = 'Barranquilla'
WHERE FirstName = 'Carlos' AND LastName = 'Gómez';
```

Listar todas las personas de la tabla.
```
Select * from Persons;
```
