#future_DS_0
#include <stdio.h>

#define MAX_CAMPAIGNS 100

typedef struct {
    char platform[20];
    char campaignName[50];
    int impressions;
    int clicks;
    int engagements;
    float cost;
    float revenue;
} Campaign;

float calculateCTR(int clicks, int impressions) {
    return impressions > 0 ? (float)clicks / impressions * 100 : 0;
}

float calculateEngagementRate(int engagements, int impressions) {
    return impressions > 0 ? (float)engagements / impressions * 100 : 0;
}

float calculateROI(float revenue, float cost) {
    return cost > 0 ? (revenue - cost) / cost * 100 : 0;
}

int main() {
    Campaign campaigns[] = {
        {"Facebook", "Summer Sale", 10000, 800, 1200, 500.0, 2000.0},
        {"Instagram", "New Arrivals", 8500, 700, 1100, 450.0, 1500.0},
        {"Facebook", "Holiday Promo", 12000, 600, 900, 700.0, 1600.0},
        {"Instagram", "Flash Deals", 9500, 950, 1300, 480.0, 2100.0}
    };
    int numCampaigns = sizeof(campaigns) / sizeof(campaigns[0]);

    printf("Ad Campaign Performance Summary:\n\n");

    for (int i = 0; i < numCampaigns; i++) {
        float ctr = calculateCTR(campaigns[i].clicks, campaigns[i].impressions);
        float engagementRate = calculateEngagementRate(campaigns[i].engagements, campaigns[i].impressions);
        float roi = calculateROI(campaigns[i].revenue, campaigns[i].cost);

        printf("Platform: %s\n", campaigns[i].platform);
        printf("Campaign: %s\n", campaigns[i].campaignName);
        printf("Impressions: %d\n", campaigns[i].impressions);
        printf("Clicks: %d | CTR: %.2f%%\n", campaigns[i].clicks, ctr);
        printf("Engagements: %d | Engagement Rate: %.2f%%\n", campaigns[i].engagements, engagementRate);
        printf("Cost: $%.2f | Revenue: $%.2f | ROI: %.2f%%\n", campaigns[i].cost, campaigns[i].revenue, roi);
        printf("-----------------------------------\n");
    }

    return 0;
}