---
title: React Hooks
date: "2020-07-21T01:19:51.310Z"
description: "Introducción a las bondades de React Hooks"
---

# Reactjs #hooks, una solución útil?

## **Cuando deberíamos usarlos?**

![Portada](./portada.jpeg)

## Intro

P`rogramación funcional` está cada vez tomando mas auge y fuerza como respuesta ante la escalabilidad, mantenimiento, entre otros beneficios desde el punto de vista técnico en el desarrollo con `javascript`.

Aunque es un paradigma ya de hace algunos buenos años. Gradualmente `facebook` entre otras industrias tecnológicas, están incluyendo está forma de programación dentro de sus stacks de javascript.

En la versión [v16.8.6](https://5d4b5feba32acd0008d0df98--reactjs.netlify.com/), los desarrolladores de facebook decidieron incluir la implementación o alternativa de los hooks. Con algunas configuraciones y actualización de tu stack podrás ya acceder a ellas.

Este artículo no está enfocado en hablar específicamente sobre los `hooks`, porque para esto hay muchos esfuerzos por parte de los developers creadores de react en agregar toda una sección sobre [Hooks](https://es.reactjs.org/docs/hooks-intro.html) en la documentación de `reactjs` (te invito a echarle una ojeadita) Sino, es proveerles un poco de mi experiencia en el uso de los hooks que son parte de la lib, así como algunos customizados.

## Contexto

Como lo establece la documentación de `reactjs` los [hooks](https://es.reactjs.org/docs/hooks-intro.html) fueron introducidos como alternativa, para acceder a las características de `reactjs` y evitar depender del ciclo del estado de una clase para poder crear un componente.

No obstante, en reactjs antes de la entrada de los `hooks`, ya se podía crear `stateless` componentes o `componentes funcionales`. Lo que no se puede desde el contexto de estos `componentes funcionales` o como se conoce en la comunidad `componentes presentacionales` es el acceder directamente al estado de un componente `statefull` o `controlado por un estado`, desde mi punto de vista, el `corazón de reactjs`.

---

## Cuestionamiento

Tengo opción de crear componentes funcionales si quiero implementar el concepto de `usabilidad` o bien crear un `estado` para poder acceder a todas las características que proponen el ciclo del estado de un componente. Entonces, para que utilizar reactjs `hooks`?

> A lo largo de todo este tiempo trabajando en el ciclo del desarrollo de un software me he dado cuenta, que no existe una solución estándar o la única solución. Mas bien, todo esta relacionado a la problemática, necesidad, recursos, presupuesto, dominio de la tecnología entre otros factores que definen a una solución como la mas viable para tu contexto.

**Considero entonces, que la respuesta ante esta inquietud es ¿en donde yo lo usaría?**

> Nota Es importante notar, que los hooks en react solo se pueden implementar en el contexto de un componente funcional puro, de lo contrario no hará ningún efecto.

Habrá muchas otras implementaciones pera esta son las dos que mas hayo común

1.  Extraer lógica común entre componentes funcionales.
2.  Extraer la lógica del componente en funciones reutilizables.

**_Para ejemplo de estos dos primeros puntos les dejo el link del_** [**_ejemplo de la misma documentación_**](https://es.reactjs.org/docs/hooks-custom.html)

En el siguiente punto, podrás notar un escenario común al usar `hooks`

3. Consumir recursos de la `API`, o bien a la capa de servicios y mapeo de servicios. (De esto hablare en otro artículo).

```js
/* eslint-disable react-hooks/rules-of-hooks */
/**
 * @name CityHook
 * @memberof module:common/hooks
 * @type {ReactHook}
 * @author boykland/clenondavis <dev@carloslenon.com>
 * @return {React} Fund Hook
 */
// #region dependencies
import { useState, useEffect } from "react"
import { useSnackbar } from "notistack"
// #endregion
// #region common
import { useStore } from "../../common/store"
import { globalAction } from "../../common/actions"
// #endregion
// #region service
import { CitiesSvc } from "../entities"
// #endregion
/**
 * @function
 * @name useCreateNewFund
 * @memberof Fund Hook
 * @description Hook to create a new fund rom funds service
 * @param {OBJECT} fundData the fund information
 * @return {OBJECT} structure for Hook context.
 */
export const useAllCitiesFromScrapp = () => {
  // TODO: remove this call instance as well as singleton pattern are implemented
  const citiesSvc = new CitiesSvc()
  const [{}, dispatch] = useStore()
  const [data, setData] = useState([])
  const { enqueueSnackbar } = useSnackbar()
  useEffect(() => {
    dispatch(globalAction.showLoadingProgress(true))
    try {
      citiesSvc.all().then(resp => {
        dispatch(globalAction.showLoadingProgress(false))

        if (Object.prototype.hasOwnProperty.call(resp, "err")) {
          enqueueSnackbar("An error occurred while loading Cities", {
            variant: "error",
          })
        } else {
          setData(resp)
        }
      })
    } catch (err) {
      enqueueSnackbar("An error occurred while loading Cities", {
        variant: "error",
      })

      dispatch(globalAction.showLoadingProgress(false))
    }
  }, [])
  return [{ citiesList: data }]
}
```

Se que hay otras implementaciones de hooks, si se te viene alguna o caso en donde crees que te sean útile ote han ayudado en tu código no dudes en compartirlo.
