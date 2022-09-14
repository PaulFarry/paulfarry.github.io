# Integration Testing with Microsoft.AspNetCore.Mvc.Testing

When you construct your paths you may need to be able to resolve them so that your tests can see the paths.

This [information](https://stackoverflow.com/a/43443712/97516) from stack overflow helped immensely to be able to view the routes without needing to add another package to the main library.

So in my tests I can use this routine to look at the routes after the application has had routes and so forth injected/updated by the testing.

```C#
using Microsoft.AspNetCore.Mvc.ActionConstraints;
using Microsoft.AspNetCore.Mvc.Infrastructure;
using Microsoft.Extensions.DependencyInjection;
using System;
using System.Collections.Generic;
using System.Linq;
using Xunit.Abstractions;

namespace Farryland.Tests
{
	internal class RouteHelper
	{
		public static List<RouteInformation> ListRoutes(IServiceProvider provider)
		{
			var actionDescriptorCollectionProvider = provider.GetRequiredService<IActionDescriptorCollectionProvider>();

			var routes = actionDescriptorCollectionProvider.ActionDescriptors.Items.Where(
			ad => ad.AttributeRouteInfo != null).Select(ad => new RouteInformation(
				ad!.AttributeRouteInfo!.Template!,
				ad.ActionConstraints?.OfType<HttpMethodActionConstraint>().FirstOrDefault()?.HttpMethods.First())
			).ToList();

			return routes;
		}

		public static void OutputRoutes(ITestOutputHelper console, List<RouteInformation> routes)
		{
			foreach (var route in routes)
			{
				console.WriteLine($"{route.Method} | {route.Name}");
			}
		}

		internal static void OutputRoutes(IServiceProvider services, ITestOutputHelper output)
		{
			var routes = ListRoutes(services);
			OutputRoutes(output, routes);
		}
	}

	internal class RouteInformation
	{
		public RouteInformation(string name, string? method)
		{
			Name = name;
			Method = method;
		}

		public string Name { get; set; }
		public string? Method { get; set; }
	}
}


```