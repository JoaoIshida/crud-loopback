
# CRUD API using LoopBack 4

This is a personal project where I'm learning how to build a simple CRUD (Create, Read, Update, Delete) API for managing products using LoopBack 4. I also used Postman to test these endpoints throughout the development process.

## Learning Journey

### Setting Up LoopBack

I started by installing LoopBack CLI globally using npm:
```bash
npm install -g @loopback/cli
```
After that, I generated a new LoopBack application:
```bash
lb4 app
```
This provided a basic structure for my project. From here, I added models, controllers, and repositories to build out the CRUD functionality.

### Defining the Product Model

I created a `Product` model, which represents the products that my API will manage. The model defines fields such as `id`, `name`, `price`, `description`, and `category`. This is what my Product model looks like:
```typescript
export class Product extends Entity {
  @property({
    type: 'string',
    id: true,
  })
  id?: string;

  @property({
    type: 'string',
    required: true,
  })
  name: string;

  @property({
    type: 'number',
  })
  price: number;

  @property({
    type: 'string',
  })
  description?: string;
}

@property({
    type: 'string',
    default: 'n/a',
  })
  category?: string;
```
I also created a repository for `Product` that interacts with the database.

### Writing the Controller

Next, I wrote a `SelectController` to handle all CRUD operations. The controller defines endpoints for creating, reading, updating, and deleting products. Here’s how I did each:

#### Creating Products
I added a POST endpoint at `/Products` to create new products:
```typescript
@post('/Products')
@response(200, {
  description: 'Product model instance',
  content: {'application/json': {schema: getModelSchemaRef(Product)}},
})
async create(
  @requestBody({
    content: {
      'application/json': {
        schema: getModelSchemaRef(Product, {
          title: 'NewProduct',
        }),
      },
    },
  })
  product: Product,
): Promise<Product> {
  return this.productRepository.create(product);
}
```

#### Reading Products
I wanted to be able to retrieve all products or count them, so I created two GET endpoints:
```typescript
@get('/Products')
@response(200, {
  description: 'Array of Product model instances',
})
async find(): Promise<Product[]> {
  return this.productRepository.find();
}
```

#### Updating Products
For updates, I used both PATCH (partial updates) and PUT (replacing the entire product):
```typescript
@patch('/Products/{id}')
async updateById(
  @param.path.string('id') id: string,
  @requestBody() product: Product,
): Promise<void> {
  await this.productRepository.updateById(id, product);
}
```

#### Deleting Products
Finally, to remove a product, I added a DELETE endpoint:
```typescript
@del('/Products/{id}')
async deleteById(@param.path.string('id') id: string): Promise<void> {
  await this.productRepository.deleteById(id);
}
```

### Testing the API with Postman

Throughout the development process, I used Postman to test each of my API endpoints. It’s a great tool for making sure your API works as expected. Here's how I used Postman to test each CRUD operation:

1. **Create Product**: In Postman, I sent a POST request to `http://localhost:3000/Products` with a JSON body:
```json
{
  "name": "Sample Product",
  "price": 19.99,
  "description": "A sample product for testing."
  "category": "Sample Category"
}
```

2. **Get All Products**: To see all the products, I sent a GET request to `http://localhost:3000/Products`.

3. **Update a Product**: I could send a PATCH request to `http://localhost:3000/Products/{id}` with updated fields like this:
```json
{
  "price": 29.99
}
```

4. **Delete a Product**: To delete a product, I just sent a DELETE request to `http://localhost:3000/Products/{id}`.

### Testing the API with Postman

Throughout the development process, I used Postman to test each of my API endpoints. It’s a great tool for making sure your API works as expected. Here's how I used Postman to test each CRUD operation:

1. **Create Product**: In Postman, I sent a POST request to `http://localhost:3000/Products` with a JSON body:
```json
{
  "name": "Sample Product",
  "price": 19.99,
  "description": "A sample product for testing."
}
```

2. **Get All Products**: To see all the products, I sent a GET request to `http://localhost:3000/Products`.

3. **Update a Product**: I could send a PATCH request to `http://localhost:3000/Products/{id}` with updated fields like this:
```json
{
  "price": 29.99
}
```

4. **Delete a Product**: To delete a product, I just sent a DELETE request to `http://localhost:3000/Products/{id}`.

### Conclusion

This project helped me get a solid understanding of how to build APIs with LoopBack 4 and test them using Postman. It's been a great learning experience, and I'm excited to continue building on top of this CRUD structure!


