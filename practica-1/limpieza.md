# Práctica 1 — Limpieza de código

## Fragmento original

Fragmento de 17 líneas seleccionado del componente `HomeScreen` compartido:

```tsx
const productImages = [
  imgFresa,         // sangria_fresa.png
  imgCitrica,       // sangria_citrica.png
  imgMango,         // Sangria_mango.png
  imgClasica,       // sangria_clasica.png
  imgFrutosRojos,   // sangria_frutosrojos.png
];

const products = t('home.products', { returnObjects: true }) as Array<any>;
// Badges de sabor para cada producto (puedes personalizar más)
const badges = [
  t('Fresa & Menta'),
  t('Cítrica'),
  t('Mango & Maracuyá'),
  t('Clásica'),
  t('Frutos Rojos'),
];
```

En el render del mismo componente, las imágenes y los badges se recuperan con `productImages[idx]` y `badges[idx]` dentro de `products.map((product, idx) => ...)`.

## Code smells identificados

- **Parallel arrays / colecciones paralelas:** `productImages`, `badges` y `products` describen el mismo catálogo en estructuras separadas y dependen del mismo orden. Agregar o reordenar un producto en una sola lista puede mostrar la imagen o el badge equivocados.
- **Acoplamiento posicional y acceso por índice:** `idx` funciona como una asociación implícita; no expresa qué imagen o badge pertenece a cada producto y puede quedar fuera de rango si las listas difieren en tamaño.
- **Uso de `any` (tipado débil):** `Array<any>` desactiva comprobaciones útiles sobre `name`, `description` y `price`, permitiendo errores que TypeScript no detecta.
- **Aserción de tipo no validada:** el `as` afirma que el resultado de i18next tiene la forma esperada, pero no la comprueba en tiempo de ejecución.

## Versión refactorizada

La propuesta usa un identificador estable por producto y mapas tipados en lugar de relacionar varias listas por índice. Para aplicarla, cada objeto de `home.products` en los archivos de traducción debe incluir su `id` estable (`fresa`, `citrica`, `mango`, `clasica` o `frutosRojos`).

```tsx
import { ImageSourcePropType } from 'react-native';

type ProductId = 'fresa' | 'citrica' | 'mango' | 'clasica' | 'frutosRojos';
type Product = {
  id: ProductId;
  name: string;
  description: string;
  price: string;
};

const imageById: Record<ProductId, ImageSourcePropType> = {
  fresa: imgFresa,
  citrica: imgCitrica,
  mango: imgMango,
  clasica: imgClasica,
  frutosRojos: imgFrutosRojos,
};

const badgeById: Record<ProductId, string> = {
  fresa: t('Fresa & Menta'),
  citrica: t('Cítrica'),
  mango: t('Mango & Maracuyá'),
  clasica: t('Clásica'),
  frutosRojos: t('Frutos Rojos'),
};

const products = t('home.products', { returnObjects: true }) as Product[];

{products.map((product) => (
  <View key={product.id} style={styles.card}>
    <Image source={imageById[product.id]} style={styles.cardImage} resizeMode="cover" />
    <Text style={styles.badgeText}>{badgeById[product.id]}</Text>
    <Text style={styles.cardTitle}>{product.name}</Text>
    <Text style={styles.cardDescription}>{product.description}</Text>
    <Text style={styles.cardPrice}>{product.price}</Text>
  </View>
))}
```

**Mejora lograda:** la relación entre producto, imagen y badge queda explícita y verificable por TypeScript; el render deja de depender del índice. La aserción de i18next sigue siendo una frontera que conviene reforzar con tipos de recursos i18next o validación del catálogo.
