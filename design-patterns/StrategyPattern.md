#### Strategy Interface

```
public interface ChargePointFilterStrategy {
     ChargePointsBO filter(GetChargePointsRequest request);
}
```

#### Strategy Implementation

```
@RequiredArgsConstructor
public class ChargePointIdFilterStrategy implements ChargePointFilterStrategy {

  @Override
  public ChargePointsBO filter(@NonNull final GetChargePointsRequest request) {
  }

}


 @RequiredArgsConstructor
 public class ChargePointStatusFilterStrategy implements ChargePointFilterStrategy {
  @Override
  public ChargePointsBO filter(@NonNull final GetChargePointsRequest request) {
  }
}

@RequiredArgsConstructor
 public class DefaultFilterStrategy implements ChargePointFilterStrategy {
   @Override
    public ChargePointsBO filter(@NonNull final GetChargePointsRequest request) {
    }
}
```

#### Strategy Factory

```
public class ChargePointFilterStrategyFactory {
public ChargePointFilterStrategy createStrategy(Map<String, String> filters) {
         if (Objects.nonNull(filters.get(ChargePointFilterAttributes.CHARGE_POINT_ID))) {
             return new ChargePointIdFilterStrategy(cpoAssetServiceSAO, cpoChargeSessionStoreServiceSAO, cpoHealthServiceSAO, metricsUtil);
         }
         if (Objects.nonNull(filters.get(ChargePointFilterAttributes.CHARGE_POINT_STATUS))) {
             return new ChargePointStatusFilterStrategy(cpoHealthServiceSAO, cpoAssetServiceSAO, cpoChargeSessionStoreServiceSAO, metricsUtil);
         }
         if (Objects.nonNull(filters.get(ChargePointFilterAttributes.LOCATION_ID))) {
             return new LocationIdFilterStrategy(cpoAssetServiceSAO, cpoChargeSessionStoreServiceSAO, cpoHealthServiceSAO, metricsUtil);
         }
         if (Objects.nonNull(filters.get(ChargePointFilterAttributes.COUNTRY_CODE))) {
             return new CountryCodeFilterStrategy(cpoAssetServiceSAO, cpoChargeSessionStoreServiceSAO, cpoHealthServiceSAO, metricsUtil);
         }
         return new DefaultFilterStrategy(cpoAssetServiceSAO, cpoChargeSessionStoreServiceSAO, cpoHealthServiceSAO, metricsUtil);
     }
}
```


#### Usage
```
final ChargePointFilterStrategy strategy = filterStrategyFactory.createStrategy(getChargePointsRequest.getFilters());
         return strategy.filter(getChargePointsRequest);
```
