# Aurm-Technologies
# Creating a complete solution for mapping bank branches based on geolocation or pincode, categorizing them by brand and facilities provided, and building APIs and a user interface is a complex project that involves various technologies and components. 
# Example code snippet for Django-based geospatial query:

from django.contrib.gis.geos import Point
from django.contrib.gis.db.models.functions import Distance

def find_branches_nearby(request):
    latitude = float(request.GET.get('latitude'))
    longitude = float(request.GET.get('longitude'))
    radius_km = 5

    user_location = Point(longitude, latitude, srid=4326)
    nearby_branches = BankBranch.objects.annotate(
        distance=Distance('location', user_location)
    ).filter(distance__lte=radius_km)

    # Serialize the result and return as JSON
    data = [{"brand": branch.brand, "facilities": branch.facilities} for branch in nearby_branches]
    return JsonResponse(data, safe=False)
