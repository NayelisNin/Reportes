# Documentción de Reportes

## C#
Lo primero que se hace es crear un formulario y luego de eso el diseño de la manera que se desee, luego se crea una carpeta dentro del proyecto que se llamará *Config*, dentro de esa carpeta se creará una clase a la cual se llamará *Conexion.cs* y dentro de esa clase se escribe lo siguiente.

``` sql
using System;
using System.Collections.Generic;
using System.Data.SqlClient;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace WindowsFormsApp2.Config
{
    class Conexion
    {
        public static SqlConnection Conectar ()
        {
            SqlConnection conexion = new SqlConnection ("server=NAYELISNIN\\SQLEXPRESS; database=BD_FacturacionPruebas; Trusted_Connection=true;");
            conexion.Open ();

            return conexion;
        }
    }
}
```

acá se coloca el nombre de nuestro servidor, la cual se hizo con SQLServer en este caso, el nombre de la base de datos y la conexión de confianza, la conexión se realizará y va a retornar los datos.

dentro del forulario se coloca lo siguiente:

```sql
 private void Form1_Load(object sender, EventArgs e)
 {
     Conexion.Conectar();
     DtGRIDViewReportes.DataSource = Index();
 }
 ```
 Esto es para poder conectar la clase con el formulario y para poder conectar el DataGridView con otra clase p[ublica llamada Index la cual es esta:
 
 ```sql
 public DataTable Index()
{
    Conexion.Conectar();

    DataTable dataTable = new DataTable();
    string sql = "select * from reportes";
    SqlCommand cmd = new SqlCommand(sql, Conexion.Conectar());

    SqlDataAdapter adapter = new SqlDataAdapter(cmd);

    adapter.Fill(dataTable);

    return dataTable;

}
```
Y de esta manera se refejan los datos de la base de datos en el DataGridView del formulario.

## PHP
Lo primero que se hará será un Index con el diseño del formulario que uno mismo desee, donde el método POST no se puede olvidar, se crearán 2 archivos nuevos que se llamrán ***conexion.php*** y ***registro.php***.

```sql
<?php
if (!empty($_POST["btnEnviar"])){


    if (!empty($_POST["ID"]) and !empty($_POST["descripcion"]) and !empty($_POST["categoria"]) and !empty($_POST["cantidad"]) and !empty($_POST["precio_unitario"]) and !empty($_POST["itbis"]) and !empty($_POST["descuento"]) and !empty($_POST["total"])){
$id=$_POST["ID"];
$descripcion=$_POST["descripcion"];
$categoria=$_POST["categoria"];
$cantidad=$_POST["cantidad"];
$PUnitario=$_POST["precio_unitario"];
$itbis=$_POST["itbis"];
$descuento=$_POST["descuento"];
$total=$_POST["total"];


$sql=$conexion->query("insert into reportes(id, descripcion, categoria, cantidad, precio_unitario, itbis, descuento, total)values('$id, $descripcion, $categoria, $cantidad, $PUnitario, $itbis, $desceunto, $total')");
if ($sql==1) {
    echo "<div class=alert alert-success>Reporte registrado correctamente</div>";
} else {
        echo "<div class=alert alert-danger>Error al intentar registrar el reporte</div>";


}


    }else{
    echo "<div class=alert alert-warning>Algunos de los campos estan vacíos</div>";
    }
}
?>
```
Lo que ocurre en esta parte es para que se registren todos los datos que se coloquen en los textbox del formulario en la tabla del mismo.

En la conexion se usa esto:
```sql
<?php
$servername = "localhost";
$username = "root";
$password = "0124";
$dbname = "bd_facturacionpruebas";


$conexion = mysqli_connect($servername, $username, $password, $dbname);


if (!$conexion) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
```
El proyecto se conecta con la base de datos para que de esta manera pueda cambiarse tanto en el formulario como en la propia base de datos.

El inicio del Index debe tomarse en cuenta la conexion con los otros archivos que son los anteriormente mencionado *conexion.php* y *registro.php*, la cual se hace de la siguiente forma:
```sql
<?php
            include "conexion.php";
            include "registro.php";
            ?>
```
De igual manera se necesita que los datos de la base de datos se reflejen en el formulario para que sea más sencillo y no haya necesidad de hacerlo de manera manual, así se utiliza:
```sql
    <?php
    include "conexion.php";
    $sql=$conexion->query("select * from reportes");
  while ($datos=$sql->fetch_object()) { ?>
         <tr>
      <td><?= $datos->id ?></td>
      <td><?= $datos->descripcion ?></td>
      <td><?= $datos->categoria ?></td>
      <td><?= $datos->cantidad ?></td>
      <td><?= $datos->precio_unitario ?></td>
      <td><?= $datos->itbis ?></td>
      <td><?= $datos->descuento ?></td>
      <td><?= $datos->total ?></td>
    </tr>
    <?php }
    ?>
```
En donde se conecta el Index con la conexion y se pide que se coloquen todos los datos de la tabla que necesitemos, que en este caso es reportes. Luego se conectan las variables con sus respectivos nombre en conjunto de las columnas que hayan en la base de datos, a lo que ambas cosas deben estar correctamente escritas para que pueda funcionar, de esta manera se logra reflejar toda la base de datos en el formulario de PHP.
