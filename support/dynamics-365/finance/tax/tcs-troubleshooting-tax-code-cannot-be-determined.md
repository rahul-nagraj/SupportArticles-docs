---
# required metadata

title: Tax code can't be determined error
description: Provides a resolution to solve the Tax code can't be determined error that occurs in Tax Calculation.
author: hangwan
ms.date: 11/21/2024
ms.custom: sap:Tax - indirect tax\Issues with advanced tax calculation

# optional metadata

ms.search.form: TaxIntegrationTaxServiceParameters
audience: Application User
# ms.devlang: 
ms.reviewer: kfend, maplnan
# ms.tgt_pltfrm: 
ms.search.region: Global
# ms.search.industry: 
ms.author: hangwan
ms.search.validFrom: 03/23/2022
ms.dyn365.ops.version: 10.0.21
---
# "Tax code cannot be determined" error in Tax Calculation

This article explains the troubleshooting steps that you can take if you receive a "Tax code cannot be determined" error in [Tax Calculation](/dynamics365/finance/localizations/global/global-tax-calcuation-service-overview) in Dynamics 365 Finance.

## Symptoms

You receive the following error message:

> Header/Lines - 1, tax code cannot be determined.

Alternatively, you find the error message in the troubleshooting file, as shown in the following example. For more information, see [How to enable debug mode for troubleshooting](tcs-troubleshooting-enable-debug-mode.md).

```jsonc
======================Tax calculation result JSON:===========================
{
    "taxDocument": {
        "Header": [
            {
                "Lines": [
                    {
                        ...
                        "Errors": [
                            {
                                "Code": "TaxSetup20001",
                                "Message": "Header/Lines - 1, tax code cannot be determined."
                            }
                        ],
                        "Adjustment": null
                    }
                ],
                "Measures": {
                    ...
                },
                ...
            }
        ]
    },
    ...
}
```

## Cause

The issue is probably occurring because the tax group and the item tax group don't intersect.

## Resolution

To solve the issue:

1. In the troubleshooting file, verify that tax group and item tax group are determined. If the values for `Tax Group` and `Item Tax Group` are blank, the tax group and item tax group aren't determined. If they're determined, the results might be incorrect.

    Here's an example of a troubleshooting file:

    ```jsonc
    ======================Tax calculation result JSON:===========================
    {
        "taxDocument": {
            "Header": [
                {
                    "Lines": [
                        {
                            "Tax Codes": {},
                            "Measures": {
                                "Tax Group": "Group A",
                                "Item Tax Group": "Group B"
                            },
                            "Adjustment": null
                        }
                    ],
                    "Measures": {
                        ...
                    },
                    ...
                }
            ]
        },
        ...
    }
    ```

2. Verify that the **Override sales tax** option on the **Setup** tab of the sales order line details is enabled.

    - If it's enabled, tax codes are determined by the `Tax group` and `Item tax group` values that you enter on the transaction line. Verify that these values are entered correctly.
    - If it isn't enabled, verify that correct values are set for the **Tax group applicability** and **Item tax group applicability** fields. For more information, see ["No matching result could be found" error in the Tax Calculation](tcs-troubleshooting-no-matching-result.md).

3. If the tax group and item tax group are determined correctly, determine whether there's any intersection for them.

    1. In [Globalization Studio](/dynamics365/finance/localizations/global/globalization-studio-overview), go to **Tax features** \> **Tax codes and groups** \> **Tax group**.

        | Line.Tax Group | Tax Codes |
        |----------------|-----------|
        | Group A        | A         |

    2. Go to **Tax features** \> **Tax codes and groups** \> **Item tax group**.

        | Line.Item Tax Group | Tax Codes |
        |---------------------|-----------|
        | Group B             | B         |

    If there's no intersection for the tax group and the item tax group, the tax code won't be determined.

## Mitigation

1. Go through each step in the [Resolution](#resolution) section of this article, and fix the setup as required. If the tax group and item tax group aren't determined correctly, see ["No matching result could be found" error in Tax Calculation](tcs-troubleshooting-no-matching-result.md).
2. If there's no intersection for the tax group and the item tax group, create a new feature version in Globalization Studio, and then fix the setup.

    - Go to **Tax features** \> **Tax codes and groups** \> **Item tax group**.

        | Line.Item Tax Group | Tax codes |
        |---------------------|-----------|
        | Group B             | A;B       |

    The tax code will be determined as **A**.
