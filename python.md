## PyTest

MagicMock with a nested property

```python
import pytest
from unittest.mock import MagicMock, patch

def create_mock_payload(json_string):
    return MagicMock(read=MagicMock(return_value=json_string))

@patch("fredcloudrunner.fredcloudapi.lambda_client.invoke")
def test_invoke_fredcloud_api_success(mock_lambda_client):
    mock_lambda_client.return_value = {
        "Payload": create_mock_payload('{ "statusCode": 201 }')
    }
    response = invoke_fredcloud_api(
        lambda_name="test_lambda_name",
        url="/v1/batches/abc123/statuses",
        payload={"statusId": 51},
    )
    assert response["statusCode"] == 201

@patch("fredcloudrunner.fredcloudapi.lambda_client.invoke")
def test_invoke_fredcloud_api_lambda_fails(mock_lambda_client):
    mock_lambda_client.side_effect = Exception("Lambda failed")
    with pytest.raises(Exception):
        invoke_fredcloud_api(
            lambda_name="test_lambda_name",
            url="/v1/batches/abc123/statuses",
            payload={"statusId": 51},
        )
```

The `beforeEach` and `afterEach` equivalent

https://stackoverflow.com/questions/22627659/run-code-before-and-after-each-test-in-py-test

```python
@pytest.fixture(autouse=True)
def run_around_tests():
    # Code that will run before your test, for example:
    files_before = # ... do something to check the existing files
    # A test function will be run at this point
    yield
    # Code that will run after your test, for example:
    files_after = # ... do something to check the existing files
    assert files_before == files_after
```
