# REST API Endpoints

-   URL paths
-   [HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)
-   Query parameters/JSON body
-   [Error status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

---

## 👍 Property

-   ### `/property/`

    -   #### `GET`: return a list of properties, default to all properties, but possibly limited by query parameters

        **Query Params**

        -   `host_id`: user ID of the host that owns the property

        **Response**

        ```json
        [
            {
                "property_id": 6532,
                "host_id": 9236,
                "address": "123 Broadway, New York, NY, United States",
                "description": "Natus id molestias corporis minima quisquam. Tempora dolor consectetur officia sequi veniam. Nostrum necessitatibus voluptatem et et. Voluptate veritatis minima ipsam aperiam eos dolor sint vero.",
                "guests_allowed": 3,
                "availability": [
                    {
                        "from": "March 1, 2025",
                        "to": "March 1, 2026",
                        "price": 500.34
                    }
                ],
                "amenities": ["WiFi", "Pool", "Air conditioning"],
                "images": ["7f46165474d11ee5836777d85df2cdab", "a4b46da106e59f424a2310cb7766366e"]
            }
        ]
        ```

    -   #### `PUT`: create a new property

        **JSON Body**

        ```json
        {
            "address": "123 Broadway, New York, NY, United States",
            "description": "Natus id molestias corporis minima quisquam. Tempora dolor consectetur officia sequi veniam. Nostrum necessitatibus voluptatem et et. Voluptate veritatis minima ipsam aperiam eos dolor sint vero.",
            "guests_allowed": 3,
            "availability": [
                {
                    "from": "March 1, 2025",
                    "to": "March 1, 2026",
                    "price": 500.34
                }
            ],
            "amenities": ["WiFi", "Pool", "Air conditioning"],
            "images": [
                {
                    "ext": "png",
                    "data": "iVBORw0KGgoAANSUhEAB4AAAAAC/kV7ZAAAAOXRFWHRTb..."
                },
                {
                    "ext": "jpg",
                    "data": "p1aS2M6tYsaJ++eUXtWzZ0uK+f/75R48++qhFvbXnLi4u..."
                }
            ]
        }
        ```

        Host ID inferred from logged in user. Images encoded in `base64` and sent along with file extensions.

        **Response**

        ```json
        {
            "property_id": 6532,
            "host_id": 9236
        }
        ```

-   ### `/property/<id>/`

    -   #### `GET`: fetch a specific property identified by its ID
        **Response**
        ```json
        {
            "property_id": 6532,
            "host_id": 9236,
            "address": "123 Broadway, New York, NY, United States",
            "description": "Natus id molestias corporis minima quisquam. Tempora dolor consectetur officia sequi veniam. Nostrum necessitatibus voluptatem et et. Voluptate veritatis minima ipsam aperiam eos dolor sint vero.",
            "guests_allowed": 3,
            "availability": [
                {
                    "from": "March 1, 2025",
                    "to": "March 1, 2026",
                    "price": 500.34
                }
            ],
            "amenities": ["WiFi", "Pool", "Air conditioning"],
            "images": ["7f46165474d11ee5836777d85df2cdab", "a4b46da106e59f424a2310cb7766366e"]
        }
        ```
    -   #### `POST`: update an existing property listing
        **JSON Body**
        ```json
        {
            "address": "123 Broadway, New York, NY, United States",
            "description": "Natus id molestias corporis minima quisquam. Tempora dolor consectetur officia sequi veniam. Nostrum necessitatibus voluptatem et et. Voluptate veritatis minima ipsam aperiam eos dolor sint vero.",
            "guests_allowed": 3,
            "availability": [
                {
                    "from": "March 1, 2025",
                    "to": "March 1, 2026",
                    "price": 500.34
                }
            ],
            "amenities": ["WiFi", "Pool", "Air conditioning"],
            "images": {
                "delete": ["7f46165474d11ee5836777d85df2cdab"],
                "add": [
                    {
                        "filename": "opq.png",
                        "data": "YYfK9AAAACXBIWXMAAC4jAAAuIwF4pT92AAEAAElEQ..."
                    }
                ]
            }
        }
        ```
    -   #### `DELETE` delete a specific property

---

## 👍 Image

-   ### `/image/`

    -   #### `PUT`: upload a list of new images

        **JSON Body**

        ```json
        [
            {
                "ext": "png",
                "data": "iVBORw0KGgoAANSUhEAB4AAAAAC/kV7ZAAAAOXRFWHRTb..."
            },
            {
                "ext": "jpg",
                "data": "p1aS2M6tYsaJ++eUXtWzZ0uK+f/75R48++qhFvbXnLi4u..."
            }
        ]
        ```

        **Response**

        ```json
        ["7f46165474d11ee5836777d85df2cdab", "a4b46da106e59f424a2310cb7766366e"]
        ```

-   ### `/images/<hash>`

    -   #### `GET`: fetch an image, encoded with the specified parameters

        **Query Params** (all optional)

        -   `width`: width of the encoded image
        -   `height`: height of the encoded image
        -   `ext`: file extension, indicating the encoding of the image; one of `jpg`, `png`, `webp`

        Only one of `width`, `height` should be specified. If both are specified, the request is invalid.

    -   #### `DELETE`: delete an image

---

## 👍 Reservation

-   ### `/reservation/`

    -   #### `GET`: return a list of reservations, limited by query parameters

        **Query Params** (at least one must be specified)

        -   `guest_id`: user ID of the guest that initiated the reservation
        -   `host_id`: user ID of the host that owns the property
        -   `status`: one of `approved`, `pending`, `denied`, `expired`
        -   `from`: start date on or before all returned reservations
        -   `to`: end date on or after all returned reservations

        **Response**

        ```json
        [
            {
                "reservation_id": 5874,
                "guest_id": 6113,
                "host_id": 9945,
                "status": "pending",
                "property_id": 6532,
                "guests": 2,
                "duration": {
                    "from": "March 3, 2025",
                    "to": "March 28, 2025"
                }
            }
        ]
        ```

    -   #### `PUT`: create a new reservation request

        **JSON Body**

        ```json
        {
            "property_id": 6532,
            "guests": 2,
            "duration": {
                "from": "March 3, 2025",
                "to": "March 28, 2025"
            }
        }
        ```

        Guest ID inferred from logged in user. The default status is `pending`.

        **Response** (the entire saved reservation object)

        ```json
        {
            "reservation_id": 5874,
            "guest_id": 6113,
            "host_id": 9945,
            "status": "pending",
            "property_id": 6532,
            "guests": 2,
            "duration": {
                "from": "March 3, 2025",
                "to": "March 28, 2025"
            }
        }
        ```

-   ### `/reservation/<id>/`

    -   #### `GET`: return a specific reservation
        **Response** (the entire saved reservation object)
        ```json
        {
            "reservation_id": 5874,
            "guest_id": 6113,
            "host_id": 9945,
            "status": "pending",
            "property_id": 6532,
            "guests": 2,
            "duration": {
                "from": "March 3, 2025",
                "to": "March 28, 2025"
            }
        }
        ```
    -   #### `POST`: modify a specific reservation
        **JSON Body**
        ```json
        {
            "status": "approved",
            "guests": 2,
            "duration": {
                "from": "March 3, 2025",
                "to": "March 28, 2025"
            }
        }
        ```