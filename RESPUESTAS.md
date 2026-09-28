# Práctica guiada MVC – EntradasMvc

## Registro de lo observado

| Momento observado | Archivo o acción responsable |
|---|---|
| El navegador solicita el formulario | `EntradasController.Index()` (GET), que devuelve la vista `Views/Entradas/Index.cshtml` |
| Se reciben el nombre y la cantidad | `EntradasController.Calcular(Cotizacion modelo)` (POST), por model binding |
| Se obtiene el descuento de Bs 25 | `Cotizacion.Descuento` en `Models/Cotizacion.cs` |
| Se presenta el total de Bs 225 | `Views/Entradas/Resultado.cshtml`, que muestra `Model.Total` |

## Preguntas

**1. ¿Qué archivo representa el modelo y qué regla de negocio contiene?**
`Models/Cotizacion.cs`. Ahí está la regla del descuento: `Cantidad >= 5 ? Subtotal * 0.10m : 0m`. También tiene el precio unitario (Bs 50), el cálculo del subtotal y del total, y las validaciones del nombre y de la cantidad.

**2. ¿Qué acción recibe los datos del formulario?**
La acción `Calcular` de `EntradasController`, marcada con `[HttpPost]`. El formulario de `Index.cshtml` apunta a ella con `asp-controller="Entradas" asp-action="Calcular"`, y ASP.NET llena el objeto `Cotizacion` con lo que escribió el usuario.

**3. ¿Por qué la vista Resultado no tiene la fórmula del descuento?**
Porque la vista solo debe mostrar datos. La regla de negocio vive en el modelo; si un día cambia el descuento se modifica en un solo lugar (`Cotizacion`) y todas las vistas reflejan el cambio sin tocarlas. Además evita repetir lógica en el HTML.

**4. ¿Qué diferencia hay entre `View(modelo)` en Index y `View("Resultado", modelo)` en Calcular?**
`View(modelo)` busca por convención una vista con el mismo nombre que la acción, o sea `Views/Entradas/Index.cshtml`. En cambio `View("Resultado", modelo)` indica el nombre de la vista explícitamente, porque la acción se llama `Calcular` y no existe una vista con ese nombre; así se usa `Resultado.cshtml` con el mismo modelo. En ambos casos solo se genera la respuesta actual, no hay redirección.

**5. ¿Por qué la validación debe existir también en el servidor?**
Porque la validación del navegador (JavaScript o atributos HTML) se puede desactivar, saltar o manipular; por ejemplo con `novalidate`, con herramientas de desarrollo o enviando la petición directamente sin usar el formulario. El servidor es el único lugar que el usuario no controla, así que ahí se debe comprobar siempre con `ModelState.IsValid` antes de procesar los datos.

## Nota sobre las capturas
Faltan las capturas que pide la entrega (resultado con 5 entradas y Visual Studio detenido en `Calcular`); hay que tomarlas ejecutando el proyecto con F5.
