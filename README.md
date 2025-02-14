RequestAccessKeys.findAll{entitlementslist.get(it)?.class?.toString()?.contains('Entitlement_values') && entitlementslist.get(it)?.entowners?.size() > 0}.size() > 0
