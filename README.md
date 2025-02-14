RequestAccessKeys.findAll{
    entitlementslist.get(it)?.class?.toString()?.contains('Entitlement_values').eq(true)
}.findAll{
    entitlementslist.get(it)?.entowners?.size().gt(0)
}.size() > 0
