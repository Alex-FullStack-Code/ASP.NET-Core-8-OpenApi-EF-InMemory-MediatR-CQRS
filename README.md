# CQRS and MediatR in ASP.NET Core - Building Scalable Systems 
### https://codewithmukesh.com/blog/cqrs-and-mediatr-in-aspnet-core/

### MediatR
```
public record ListProductsQuery : IRequest<List<ProductDto>>;
Every Query / Command object would inherit from 
IRequest<T> interface of the MediatR library, 
where T is the object to be returned. 
In this case, we will return List<ProductDto>.

Next, we need handlers for our Query. 
This is where the ListProductsQueryHandler comes into the picture. 
Note that whenever our LIST endpoint is hit, 
this handler will be triggered.
public class ListProductsQueryHandler(AppDbContext context) : IRequestHandler<ListProductsQuery, List<ProductDto>>
{
    public async Task<List<ProductDto>> Handle(ListProductsQuery request, CancellationToken cancellationToken)
    {
        return await context.Products
            .Select(p => new ProductDto(p.Id, p.Name, p.Description, p.Price))
            .ToListAsync();
    }
}
To the primary constructor of this handler, 
we will inject the AppDbContext instance, for data access. 
Also, all the handlers would implement 
the IRequestHandler<T, R> where T is the incoming request 
(which in our case would be the Query itself), 
and R would be the response, which is a list of products.

This interface would want us to implement the Handle method. 
We simply use the DB Context to project 
the Product domain into a list of DTOs with 
ID, Name, Description, and Price. 
This list would be returned.
```




