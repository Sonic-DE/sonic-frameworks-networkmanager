include_directories (
    ${CMAKE_SOURCE_DIR}/src/settings
)

include(ECMAddTests)

find_package(Qt6 ${REQUIRED_QT_VERSION} NO_MODULE REQUIRED Test)
set_package_properties(Qt6Test PROPERTIES PURPOSE "Required for autotests")

ecm_add_test(managertest.cpp LINK_LIBRARIES Qt6::Test KF6NetworkManagerQt_static PkgConfig::NetworkManager fakeNetwork)
ecm_add_test(settingstest.cpp LINK_LIBRARIES Qt6::Test KF6NetworkManagerQt_static PkgConfig::NetworkManager fakeNetwork)
ecm_add_test(activeconnectiontest.cpp LINK_LIBRARIES Qt6::Test KF6NetworkManagerQt_static PkgConfig::NetworkManager fakeNetwork)
# Each test registers the fake NetworkManager service on the session bus; two at once take each other's place
set_tests_properties(managertest settingstest activeconnectiontest PROPERTIES RESOURCE_LOCK fake-networkmanager)

add_subdirectory(settings)
