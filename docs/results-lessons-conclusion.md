# results, lessons and final conclusion

## 13. Results
### Unstable ADC

- backend selection changes per request

- inconsistent bucket visibility

- TCP resets observed

- unpredictable S3 behavior

## Correct ADC Configuration

- consistent backend affinity

- stable bucket visibility

- no repeated TCP resets

- predictable client behavior

## 14. Key Lessons

When deploying S3 object storage behind an ADC:

- Correct request routing is critical.

- Important design principles:

  1. Ensure backend state consistency

  2. Use persistence when required

   
  3. Avoid aggressive connection timeout settings

  4. Validate behavior using real S3 operations

  5. Monitor traffic using packet captures

Incorrect load balancing can easily introduce subtle and difficult to diagnose failures.

## 15.  Final Conclusion

This lab demonstrates that an incorrectly configured ADC can reproduce symptoms often seen in production S3 environments, including:

- inconsistent object visibility

- metadata failures

- TCP resets

By applying correct persistence and stable connection handling, the behavior becomes predictable and stable.

This highlights the importance of careful ADC design when deploying S3-compatible storage platforms.