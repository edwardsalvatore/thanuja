RequestAccessKeys.findAll { 
    entitlementslist.get(it)?.class?.toString()?.contains('Entitlement_values')
}.findAll { 
    entitlementslist.get(it)?.entowners?.size() > 0
}.size() > 0
