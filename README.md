RequestAccessKeys.findAll { 
    def entitlement = entitlementslist.get(it)
    entitlement?.class?.toString()?.contains('Entitlement_values') && 
    entitlement?.entowners?.size() > 0
}.size() > 0
